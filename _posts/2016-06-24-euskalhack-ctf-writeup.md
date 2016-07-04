---
title: Euskalhack CTF Writeup
date: '2016-06-24 21:17:00'
layout: post
tags:
- ctf
- euskalhack
---
## DNS codified (50pts)

`Una captura un tanto sospechosa ...` 
translates to *a suspicious capture*:

Download pcap:
https://archive.org/download/theinternet_201606/theinternet.zip

Analyzing the file with wireshark i found this line:

```wireshark
63	96.986931	104.131.38.172	91.200.40.69	HTTP	163	GET /secure-atom128c-online HTTP/1.1 
```

something or somebody used the site http://crypo.bz.ms/secure-atom128c-online to encrypt something.

for example the subdomain name of

`A5r1AJ6fDGhiAguUAGufHJX1HGkGfJ6eAqCC.exfil.identificar.me`

decrypting it using the same page we get:

`FLAGISHAVENODNSWHATMDOING = A5r1AJ6fDGhiAguUAGufHJX1HGkGfJ6eAqCC`

## This is Scada (100pts)

> Operators of a water treatment plant have detected malfunctions in several PLCs and HMIs. Because of previous security incidents a monitoring equipment has been attached to a switch, which has been configured to receive all traffic inside the treatment plant in the aim to analyze it. 

> A pcap file is facilitated to the investigators, so is a map with PLCs variables and a screenshot from the HMI to visualize the industrial control process.

![]({{ site.baseurl }}/forestryio/images/Mapa_variables_PLC.png)

![]({{ site.baseurl }}/forestryio/images/Pantalla_HMI.png)

Download pcap: [euskalhack_industrial_forensics_challenge.rar](https://archive.org/download/euskalhack_industrial_forensics_challenge/euskalhack_industrial_forensics_challenge.rar)

* Find the IP of the HMI
* Find the IP of the PLC
* Which protocol do they use to communicate with eachother?

HMI `172.16.136.134` and PLC `172.16.136.133`in Network Miner:

![]({{ site.baseurl }}/forestryio/images/scada_hosts.png)

Example Modbus/TCP Traffic in Wireshark:

![]({{ site.baseurl }}/forestryio/images/2016-06-25 22_58_54-euskalhack_industrial_forensics_challenge.pcap.png)

## Don't stop me now (300pts)

Find the secrets of user alpacino. [Download Virtualbox VM](https://mega.nz/#!DkJixThJ!KQHnMtYcIWXeRPEFAg0-2lq1d0ihDHcV1u2D6qcLQ9E).

First look at the VM presents us with a logged in user (alpacino) executing two tasks:

![]({{ site.baseurl }}/forestryio/images/euskalhack_ctf_3-1.png)

Trying default password reveal nothing, neither does a scan with nmap, but there are snapshots in the VM folder. Which gives me the idea to extract the VM's memory:

`VBoxManage.exe debugvm "WinXP" dumpvmcore --filename=memory.raw`

![]({{ site.baseurl }}/forestryio/images/euskalhack_ctf_5.png)

We now have a memory dump and can proceed to inspect it usingn the [volatility framework](https://github.com/volatilityfoundation/volatility). First of all we need to determine the profile:

```bash
$ volatility imageinfo -f memory.raw`
```

![]({{ site.baseurl }}/forestryio/images/euskalhack_ctf_6.png)

The suggested VM profile is WinXPSP2x86 which we can use to get a list of all running processes:

```bash
$ volatility --profile=WinXPSP2x86 -f memory.raw pslist
```

![]({{ site.baseurl }}/forestryio/images/euskalhack_ctf_7.png)

So alpacino is running TrueCrypt and the KeePass Password Manager. It comes to my mind, that using the password manager there may be some password in the clipboard. And indeed there is something...

![]({{ site.baseurl }}/forestryio/images/euskalhack_ctf_8.png)

...but trying `las rosas son Rojas y el mar es Violeta 123..` as a password doesn't get me any further. So TrueCrypt is next and fortunally volatility comes with a module called [truecryptsummary](https://fossies.org/dox/volatility-2.5/classvolatility_1_1plugins_1_1tcaudit_1_1TrueCryptSummary.html):

```bash
$ volatility --profile=WinXPSP2x86 -f memory.raw truecryptsummary
```

![]({{ site.baseurl }}/forestryio/images/euskalhack_ctf_9.png)

Now you can go either way:

* Enter the XP Machine using [konboot](http://piotrbania.com/all/kon-boot/) (grab old/free version) and bypassing the admin password
* or (if you want to leave the machine untouched) extract the container. The method is described in this presentation: [Mastering Truecrypt - Windows 8 and Server 2012 Memory Forensics](http://downloads.volatilityfoundation.org/omfw/2013/OMFW2013_Ligh.pdf) (page 19)
 
I'll go the the fastest way which is bypassing the admin login:

![]({{ site.baseurl }}/forestryio/images/konboot.png)

![]({{ site.baseurl }}/forestryio/images/euskalhack_ctf_11.png)

![]({{ site.baseurl }}/forestryio/images/euskalhack_ctf_12.png)

**Further reading:**

Memory Extraction of Truecrypt keys is a known flaw and has been discussed in detail:

[TrueCrypt Master Key Extraction And Volume Identification](https://news.ycombinator.com/item?id=7064188)
