---
title: Setup AlgoVPN in Windows 10
date: 2017-10-23 00:00:00 +0000
layout: post
tags:
- setup
- windows
- algovpn
- opsec
---


[AlgoVPN](https://github.com/trailofbits/algo) is a self-hosted personal VPN server designed for ease of deployment and security. Algo automatically deploys an on-demand VPN service in the cloud that is not shared with other users, relies on only modern protocols and ciphers, and includes only the minimal software you need.

**Features**

* Supports only IKEv2 with strong crypto: AES-GCM, SHA2, and P-256

* Generates Apple profiles to auto-configure iOS and macOS devices

* Includes a helper script to add and remove users

* Blocks ads with a local DNS resolver (optional)

* Sets up limited SSH users for tunneling traffic (optional)

* Based on current versions of Ubuntu and strongSwan

* Installs to [DigitalOcean](https://m.do.co/c/1d11b814bb23), Amazon EC2, Microsoft Azure, Google Compute Engine, or your own server

**Anti-features**

* Does not support legacy cipher suites or protocols like L2TP, IKEv1, or RSA

* Does not install Tor, OpenVPN, or other risky servers

* Does not depend on the security of TLS

* Does not require client software on most platforms

* Does not claim to provide anonymity or censorship avoidance

* Does not claim to protect you from the FSB, MSS, DGSE, or FSM

Sounds good? It did to me and so I decided to try it out. Installation on Linux is pretty straight forward and if you're familiar with Linux you shouldn't have any problem. I decided to try it out for Windows 10 since and it was surprisingly easy to set up:

### 1. Get a server:

I chose a DigitalOcean Droplet (1GB RAM/30GB Disk/2TB BW). You may choose a smaller one and it will work just fine. Actually you can [click my referral link](https://m.do.co/c/1d11b814bb23) and **get a $10 discount**.

![]({{ site.baseurl }}/forestryio/images/2017-10-18%2012_18_05-DigitalOcean%20-%20Create%20Droplets%20-%20Opera.png)

Choose an operating system, datacenter location and hostname:

![]({{ site.baseurl }}/forestryio/images/2017-10-18%2012_16_01-DigitalOcean%20-%20Create%20Droplets%20-%20Opera.png)

### 2. Setup Server

[Add the ansible repository](http://docs.ansible.com/ansible/latest/intro_installation.html#latest-releases-via-apt-debian) to your sources and update the system.

```
apt-get update &amp;amp;amp;amp;amp;amp;amp;amp;&amp;amp;amp;amp;amp;amp;amp;amp; apt-get upgrade --yes
apt-get install ansible git
git clone https://github.com/trailofbits/algo
cd algo
python -m virtualenv env
source env/bin/activate

```

Edit `config.cfg` and add the username(s) you need. Save and start `./algo`. Select "Existing Ubuntu Server" and accept default answers for the rest of the <span style="font-size: 1rem;">questions:</span>

![]({{ site.baseurl }}/forestryio/images/2017-10-18%2012_47_35%20-%20PuTTY.png)

Client configuration files are generated in algo/configs/$IP/

![]({{ site.baseurl }}/forestryio/images/2017-10-18%2012_53_04-certs%20-%20PuTTY.png)

### 3. Setup Windows Client

Grab cacert.pem, $user.p12 and windows_$user.ps1 with Filezilla or WinSCP:

![]({{ site.baseurl }}/forestryio/images/2017-10-18%2012_57_02-Gestor%20de%20sitios.png)

Use Certificate Manager to import the CA cert:

![]({{ site.baseurl }}/forestryio/images/2017-10-18%2013_10_35-certlm%20-%20%5BCertificados%20-%20Equipo%20local_Entidades%20de%20certificaci%C3%B3n%20ra%C3%ADz%20de%20confian.png)

![]({{ site.baseurl }}/forestryio/images/2017-10-18%2013_13_11-NALLAM%20FORMACION%20-%20Campus%20Nallam%20-%20EXPERTO%20EN%20CIBERSEGURIDAD.png)

Set Execution-Policy to unrestricted (don't forget to set it back to restricted afterwards)...

![]({{ site.baseurl }}/forestryio/images/2017-10-18%2013_16_40-Administrador_%20Windows%20PowerShell.png)

...and run the following command as Administrator to configure the VPN connection:

`powershell -ExecutionPolicy ByPass -File windows_{username}.ps1 Add`

![]({{ site.baseurl }}/forestryio/images/2017-10-18%2013_17_22-Administrador_%20Windows%20PowerShell.png)

For restricted user accounts you'll have to create an IKEv2 connection manually, import the cert into personal certificates and activate stronger ciphers on it via the following PowerShell script:

`Set-VpnConnectionIPsecConfiguration -ConnectionName "Algo" -AuthenticationTransformConstants GCMAES128 -CipherTransformConstants GCMAES128 -EncryptionMethod AES128 -IntegrityCheckMethod SHA384 -DHGroup ECP256 -PfsGroup ECP256`

