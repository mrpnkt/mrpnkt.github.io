---
title: Writeup Bulldog 1
date: 2017-10-27 00:00:00 +0000
layout: post
tags:
- walkthrough
- writeup
- vulnhub
---
Since I was somewhat bored and hadn't done a vulnerable machine for quite some time I had a look at [vulnhub](https://www.vulnhub.com/) and saw a new machine:

**Bulldog: 1 - [Info & Download](https://www.vulnhub.com/entry/bulldog-1,211/)**

![]({{ site.baseurl }}/forestryio/images/2017-10-24%2011_57_54-Bulldog%20Industries%20-%20Opera-1.png)

If you're going to try, listen to the authors recommendations and don't try to run it in VMWare. Doesn't work, believe me.

<blockquote>Bulldog Industries recently had its website defaced and owned by the malicious German Shepherd Hack Team. Could this mean there are more vulnerabilities to exploit? Why don't you find out? :)<br>This is a standard Boot-to-Root. Your only goal is to get into the root directory and see the congratulatory message, how you do it is up to you!<br>Difficulty: Beginner/Intermediate, if you get stuck, try to figure out all the different ways you can interact with the system. That's my only hint ;)</blockquote>

With the machine up and running I started poking around and enumerating the system:

Browsing the website http://192.168.33.101/ (set entry to bulldogindustries.com in /etc/hosts/ if you like) i found a "hacked" robots.txt with a reference to the malicious German Shepherd Hack Team:

![]({{ site.baseurl }}/forestryio/images/2017-10-24%2011_59_20-Kali-Linux-Light-2017.2-vm-amd64%20-%20VMware%20Workstation.png)

Through dirbusting directories i found some directories and a note from the dev team:

```
/
/admin/
/admin/auth/
/dev/
/admin/login/
/admin/logout/
/admin/auth/group/
/dev/shell/
/admin/auth/user/
/notice
/static/js/jquery.min.js
/static/js/bootstrap.js

```

some emails:

```
Team Lead: alan@bulldogindustries.com Team Lead: Alan Brooke
Back-up Team Lead: william@bulldogindustries.com

Front End: malik@bulldogindustries.com
Front End: kevin@bulldogindustries.com
Back End: ashley@bulldogindustries.com
Back End: nick@bulldogindustries.com
Database: sarah@bulldogindustries.com

```

the name of the CEO (Winston Churchy) and maybe some hints about the technology (Django, MongoDB ("not fully installed")) and how the site got hacked in the first place (Smelly cow = dirty cow, ¿Clam shell?). Portscanning didn't reveal much (ssh on port 23, open port on 8080).

The server response revealed a <span style="font-size: 1rem;">WSGIServer/0.1 Python/2.7.12</span>

and some filtered mongodb ports:

```
27017/tcp filtered mongod
27018/tcp filtered mongod
27019/tcp filtered mongod
28017/tcp filtered mongod

```

wrong way :(

I tried bruteforcing the admin panel using [top-500-worst-passwords](https://downloads.skullsecurity.org/passwords/500-worst-passwords.txt.bz2) and trying for backend users ashley and nick:

![]({{ site.baseurl }}/forestryio/images/2017-10-24%2012_40_56-Kali-Linux-Light-2017.2-vm-amd64%20-%20VMware%20Workstation-1.png)

Although the free edition of [burpsuite](https://portswigger.net/burp) took forever...

![]({{ site.baseurl }}/forestryio/images/2017-10-25%2015_02_24-Kali-Linux-Light-2017.2-vm-amd64%20-%20VMware%20Workstation.png)

I had luck bruteforcing Nicks' password (turns out I could've used this [dog-related dictionary](https://packetstormsecurity.com/files/32095/dogs.gz.html) to save some time), but... bummer Nick is a just regular staffer with no admin rights:

![]({{ site.baseurl }}/forestryio/images/2017-10-25%2015_03_36-Kali-Linux-Light-2017.2-vm-amd64%20-%20VMware%20Workstation.png)

Learning about [pentesting django](https://es.slideshare.net/levigross/pentesting-django-and-rails) and [some more stuff](https://github.com/nixawk/pentest-wiki/blob/master/2.Vulnerability-Assessment/Database-Assessment/mongodb/MongoDB%20Pentesting%20for%20Absolute%20Beginners.pdf) about mongodb (Free eBook: MongoDB Pentesting for Absolute Beginners) I wouldn't need for this machine, I found that an authenticated user (in my case Nick) has access to a limited webshell:

![]({{ site.baseurl }}/forestryio/images/2017-10-26%2010_21_38-Kali-Linux-Light-2017.2-vm-amd64%20-%20VMware%20Workstation.png)

Cool!

Although the shell is restricted to a few commands and throws a warning when trying to concatenate with a semicolon, there is no restriction to using ampersands. So... I had a look at everything accessible to the django user.

The system:

```
cat /proc/version

Linux version 4.4.0-87-generic (buildd@lcy01-31) (gcc version 5.4.0 20160609 (Ubuntu 5.4.0-6ubuntu1~16.04.4) ) 

```

The users:

```
cat /etc/passwd | cut -d: -f1

Usernames:

root
[snip]
bulldogadmin
django
sshd

```

A hidden note written by Ashley:

```
cat /home/bulldogadmin/.hiddenadmindirectory/note

Nick,

I'm working on the backend permission stuff. Listen, it's super prototype but I think it's going to work out great. Literally run the app, give your account password, and it will determine if you should have access to that file or not! 

It's great stuff! Once I'm finished with it, a hacker wouldn't even be able to reverse it! Keep in mind that it's still a prototype right now. I am about to get it working with the Django user account. I'm not sure how I'll implement it for the others. Maybe the webserver is the only one who needs to have root access sometimes?

Let me know what you think of it!

-Ashley

```

Pretty sure ashley was talking about this one:

`-rw-r--r-- 1 bulldogadmin bulldogadmin 8.6K Aug 26 03:18 customPermissionApp`

A secret key :o in settings.py:

```
# SECURITY WARNING: keep the secret key used in production secret!
SECRET_KEY = '%9a3ph3iwk$v*_#x4ejg8(t5(qll0fl8q8&amp;amp;u+o_g$yi83d*riq'

```

and a database (db.sqlite3), which I copied to /static/js to download and [dumped](https://stackoverflow.com/questions/75675/how-do-i-dump-the-data-of-some-sqlite3-tables) the contents:

```
sqlite3 db.sqlite3 .dump | grep '^INSERT'

[snip]

CREATE TABLE IF NOT EXISTS "auth_user" ("id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, "password" varchar(128) NOT NULL, "is_superuser" bool NOT NULL, "username" varchar(30) NOT NULL UNIQUE, "first_name" varchar(30) NOT NULL, "last_name" varchar(30) NOT NULL, "email" varchar(254) NOT NULL, "is_staff" bool NOT NULL, "is_active" bool NOT NULL, "date_joined" datetime NOT NULL, "last_login" datetime NULL);
INSERT INTO auth_user VALUES(1,'pbkdf2_sha256$20000$9k0TYJltYWk5$rE0aQA4DGFxEjBhBH0BEJhFsF2Jx63690a8VGE/9a+c=',1,'admin','','','admin@bulldogindustries.com',1,1,'2017-08-18 22:42:27.888865','2017-08-19 06:14:22.419010');
INSERT INTO auth_user VALUES(3,'pbkdf2_sha256$20000$AvMG3SPMFdWk$br74kiJcinPLKkqG+i9G+2MavMVW9IXDl2TSeQ5My+A=',0,'alan','','','',1,1,'2017-08-19 05:57:32',NULL);
INSERT INTO auth_user VALUES(4,'pbkdf2_sha256$20000$LgCCwZ1qFhSK$xv2NHkto76GEp11lXNUFPsiolvoV8c8R/PRl2/XccX0=',0,'william','','','',1,1,'2017-08-19 06:02:07',NULL);
INSERT INTO auth_user VALUES(5,'pbkdf2_sha256$20000$IjvfpwWo8tw9$9PZgYsZCcRz3dLVU/4TnXa9i2VcybBnBedCyNjk3Sak=',0,'malik','','','',1,1,'2017-08-19 06:02:50',NULL);
INSERT INTO auth_user VALUES(6,'pbkdf2_sha256$20000$ooicdWHyxlTk$oNMBAZDKarihoPIzzYAhDq+4cR8JtJGt9JQIA8q1SX8=',0,'kevin','','','',1,1,'2017-08-19 06:03:22',NULL);
INSERT INTO auth_user VALUES(7,'pbkdf2_sha256$20000$nmdkvhu3yqa9$8VSo44h9fXYj6FLavolYDZ7P5PhFBLeKBmLkBlNpGTk=',0,'ashley','','','',1,1,'2017-08-19 06:04:18',NULL);
INSERT INTO auth_user VALUES(8,'pbkdf2_sha256$20000$Cto2iGhTaD7o$BrQ4VQiA/gK3pMtijCDT30k7m0opyDrwjlOiEmAmtIc=',0,'nick','','','',1,1,'2017-08-19 06:06:08','2017-10-24 21:25:02.575798');
INSERT INTO auth_user VALUES(9,'pbkdf2_sha256$20000$lA6iOt4XGXLw$0VHpbYjNiFN4CnHisuB+bFh72A6sn03Q+d34Laj7jkM=',0,'sarah','','','',1,1,'2017-08-19 06:06:56',NULL);
CREATE TABLE IF NOT EXISTS "django_session" ("session_key" varchar(40) NOT NULL PRIMARY KEY, "session_data" text NOT NULL, "expire_date" datetime NOT NULL);
INSERT INTO django_session VALUES('7c9ks1ywspnb2mu500z63zp178eo1zrh','YjcyYTMwNzY4Mzk2MTFmMmQyYmE1MGZhMzA2MzNmYjdiODA3NGEwMDp7Il9hdXRoX3VzZXJfaGFzaCI6IjliMThhNzgyNjI4YjEzZDFhY2E4YjAwMjA3MTIwMWUxODUzNjMyMTIiLCJfYXV0aF91c2VyX2JhY2tlbmQiOiJkamFuZ28uY29udHJpYi5hdXRoLmJhY2tlbmRzLk1vZGVsQmFja2VuZCIsIl9hdXRoX3VzZXJfaWQiOiI4In0=','2017-09-02 06:37:06.605011');
INSERT INTO django_session VALUES('exuboafi4l2garfjmb92z041pspfodtl','YjcyYTMwNzY4Mzk2MTFmMmQyYmE1MGZhMzA2MzNmYjdiODA3NGEwMDp7Il9hdXRoX3VzZXJfaGFzaCI6IjliMThhNzgyNjI4YjEzZDFhY2E4YjAwMjA3MTIwMWUxODUzNjMyMTIiLCJfYXV0aF91c2VyX2JhY2tlbmQiOiJkamFuZ28uY29udHJpYi5hdXRoLmJhY2tlbmRzLk1vZGVsQmFja2VuZCIsIl9hdXRoX3VzZXJfaWQiOiI4In0=','2017-09-09 02:48:23.352017');
INSERT INTO django_session VALUES('7toi97v6ux7duk4o9mzu7v34oiwhlexs','YjcyYTMwNzY4Mzk2MTFmMmQyYmE1MGZhMzA2MzNmYjdiODA3NGEwMDp7Il9hdXRoX3VzZXJfaGFzaCI6IjliMThhNzgyNjI4YjEzZDFhY2E4YjAwMjA3MTIwMWUxODUzNjMyMTIiLCJfYXV0aF91c2VyX2JhY2tlbmQiOiJkamFuZ28uY29udHJpYi5hdXRoLmJhY2tlbmRzLk1vZGVsQmFja2VuZCIsIl9hdXRoX3VzZXJfaWQiOiI4In0=','2017-09-11 22:37:35.697643');
INSERT INTO django_session VALUES('kknxoaa81vj29zqbumpnwky2eymbwpnq','YjcyYTMwNzY4Mzk2MTFmMmQyYmE1MGZhMzA2MzNmYjdiODA3NGEwMDp7Il9hdXRoX3VzZXJfaGFzaCI6IjliMThhNzgyNjI4YjEzZDFhY2E4YjAwMjA3MTIwMWUxODUzNjMyMTIiLCJfYXV0aF91c2VyX2JhY2tlbmQiOiJkamFuZ28uY29udHJpYi5hdXRoLmJhY2tlbmRzLk1vZGVsQmFja2VuZCIsIl9hdXRoX3VzZXJfaWQiOiI4In0=','2017-11-07 16:47:19.297983');
[snip]

```

Session keys and salted/hashed passwords including the one for the admin user.

I lost some time messing with hashcat trying to find a way to bruteforce. Turns out, that's not as easy as I thought. Here are some links as a reference if you want to try:

```
https://www.djangoproject.com/weblog/2013/sep/15/security/
https://blog.codinghorror.com/hacker-hack-thyself/
https://github.com/xros/py_django_crack
http://passlib.readthedocs.io/en/stable/lib/passlib.hash.django_std.html
https://crypto.stackexchange.com/questions/18173/how-long-does-it-take-to-crack-pbkdf2
https://autohotkey.com/board/topic/108950-pbkdf2-makes-passwords-harder-to-crack/
```

I decided to take a shortcut, change the admin password and serve the modified database from the attacking machine:

```
sqlite3 db.sqlite3 < output.sql
python -m SimpleHTTPServer 80
```

