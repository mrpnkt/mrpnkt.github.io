---
title: 'Prevenir la ejecución de #BadRabbit'
date: 2017-10-25 00:00:00 +0000
layout: post
tags:
- homework
- physec
---


Un nuevo ataque de ransomware se está extendiendo como un reguero de pólvora por Europa y ya ha afectado a más de 200 organizaciones importantes, principalmente en Rusia, Ucrania, Turquía y Alemania, en las últimas horas.

![]({{ site.baseurl }}/forestryio/images/bad_rabbit_ransomware_01.png)

Denominado "Bad Rabbit", se informa que es un nuevo ataque de ransomware específico de Petya contra las redes corporativas, que exige 0.05 bitcoins (~ $ 285) como rescate de las víctimas para desbloquear sus sistemas.

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">Video of <a href="https://twitter.com/hashtag/BadRabbit?src=hash&amp;ref_src=twsrc%5Etfw">#BadRabbit</a> in action, from <a href="https://twitter.com/anyrun_app?ref_src=twsrc%5Etfw">@anyrun_app</a> (doesn't show reboot) <a href="https://t.co/3WpTj7f6qE">pic.twitter.com/3WpTj7f6qE</a></p>— Beaumont Porg, Esq. (@GossiTheDog) <a href="https://twitter.com/GossiTheDog/status/922858264534142976?ref_src=twsrc%5Etfw">October 24, 2017</a></blockquote>
<script type="null"></script>

De acuerdo con un [análisis inicial proporcionado por Kaspersky](https://securelist.com/bad-rabbit-ransomware/82851/), el ransomware se distribuyó mediante ataques de [descarga drive-by](http://www.eset-la.com/centro-amenazas/1792-drive-by-download-infeccion-web), utilizando un instalador falso de reproductores de Adobe Flash para atraer a las víctimas a instalar malware.

### Prevenir la ejecución del ransomware:

Abrir powershell (Ejecutar como administrador) y crear los siguientes ficheros:

```
c:\windows\infpub.dat
c:\Windows\cscc.dat

```

![]({{ site.baseurl }}/forestryio/images/2017-10-25%2008_29_23-Administrador_%20Windows%20PowerShell.png)

y [quitar los permisos heredados](https://twitter.com/0xAmit/status/922911491694694401):

![]({{ site.baseurl }}/forestryio/images/2017-10-25%2008_27_58-Windows-1.png)

**Más información:**

* <a href="http://www.hackplayers.com/2017/10/badrabbit-que-es-lo-que-hay-que-saber-de-momento.html" style="font-size: 1rem; background-color: rgb(255, 255, 255);">Hackplayers - Qué es lo que hay que saber (de momento) de la nueva amenaza de #ransomware #BadRabbit</a>

* <a href="https://www.csoonline.com/article/3234691/security/badrabbit-ransomware-attacks-multiple-media-outlets.html" style="font-size: 1rem; background-color: rgb(255, 255, 255);">BadRabbit ransomware attacks multiple media outlets</a>

* [Microsoft: Ransom:Win32/Tibbar.A](https://www.microsoft.com/en-us/wdsi/threats/malware-encyclopedia-description?Name=Ransom:Win32/Tibbar.A)

* [roycewilliams/badrabbit-info.txt](https://gist.github.com/roycewilliams/a723aaf8a6ac3ba4f817847610935cfb)

&amp;
