---
title: "#moocHackingMU - Semana 1 - Tarea 3"
date: '2016-09-20 17:42:00'
layout: post
---
### TAREA 3: Una sencilla práctica sobre criptografía

> En esta tarea te proponemos el intercambio de un archivo cifrado y firmado digitalmente. Para ello será necesario trabajar en grupos de al menos dos personas. A grandes rasgos, cada integrante del grupo generará un archivo de texto (mensaje), lo cifrará con la clave pública de un tercero y a su vez, compartirá el mensaje cifrado con el propietario de la clave pública para que éste lo descifre utilizando su clave privada.

Para esta tarea usamos el [GNU Privacy Assistant (gpa)](https://www.archlinux.org/packages/community/x86_64/gpa/) junto con [Claws Mail](https://wiki.archlinux.org/index.php/Claws_Mail_(Espa%C3%B1ol)) como gestor de correo. 

Primero hay que activar el módulo de gpg dentro de claws:

![](http://i.imgur.com/vjxQURb.png)

![](http://i.imgur.com/ale1Qj8.png)

Generar una llave nueva:

![](http://i.imgur.com/5lt3pPs.png)

...y intercambiar las llaves públicas:

![](http://i.imgur.com/0SSwpMF.png)

![](http://i.imgur.com/Xqpl756.png)

![](http://i.imgur.com/b66RbsJ.png)

ahora podemos cifrar el mensaje:

![](http://i.imgur.com/yKrHA3m.png)

...y el destinatario puede descifrarla:

![](http://i.imgur.com/MDSf0ad.png)


