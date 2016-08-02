---
title: Writeup labs.gf0s.com
date: '2016-08-02 17:45:00'
layout: post
---

# Writeup labs.gf0s.com

## HackLab #1

> **Escenario del laboratorio:** Imagina en 3D, una emprea que se dedica al diseño de drones de última generación. Recientemente ha realizado una investigación que ha dado como resultado un prototipo de lo que considerán será el dron más potente de nuestros tiempos. Si el diseño de este prototipo llegará a ser expuesto en Internet antes de la fecha programada, causaría una grán pérdida economica para Imagina en 3D.  Para evitar que esto ocurra, has sido contratado con el objetivo evaluar los controles que estan implementados y demostrar si la seguridad puede ser vulnerada.

Tenemos 2 objetivos:

1. Intentar obtener el diseño del prototipo del dron.
2. Dejar evidencia (un archivo .txt)    

La única información que nos dan es el nombre (Alejandro Quero) y el [Facebook](https://www.facebook.com/profile.php?id=100010301916909) del administrador.

Así que empezamos a cotillear por su cuenta que nos dará algunas cuantas pistas sobre la política de contraseñas y la infraestructura de la red (interna) de la empresa:

Fecha de nacimiento: **11/08/1980**
Incorporación en la empresa: **9/9/2015**

Política de contraseñas:

![]({{ site.baseurl }}/forestryio/images/11836794_121354761551254_7265632735719379331_n.jpg)

* Para contraseñas de dispositivos accesibles vía red: **únicamente números, 7 dígitos**
* Para contraseñas locales (PDF,doc,zip,...): **letras minúsculas y números, 6 dígitos**

Infraestructura de red:

![]({{ site.baseurl }}/forestryio/images/11218839_119723698381027_7030869511675839566_n.jpg)

* Un pantallazo del login al servidor ftp (IP: **52.10.103.130**)
* Dos PDF con un mapa de la red WAN y LAN

También encontramos metadatos en los ficheros PDF (impresora/software usado, nombre de usuario, ...), pero en principio los datos de red, el usuario (**aquero**) y las pistas de contraseñas son suficiente para lanzar un ataque de fuerza bruta contra el servicio ftp. Como sabemos la longitud de la contraseña podemos optimizar el diccionario usado con crunch:

`crunch 7 7 012345678 > diccionario.dic`

lo que genera un archivo usando números del 0 al 9 con 7 caracteres que podemos usar con hydra:

`hydra -l aquero -P diccionario.dic ftp://52.10.103.130`

Sabemos que el servidor esta en la nube amazonaws, así que atacando desde otro servidor en amazonaws nos iría un poco mas rápido, pero aún haciéndolo desde una conexión lenta no debería tardar mas de 15 minutos. Se puede optimizar el diccionario teniendo en cuenta la fecha del nacimiento para ahorrar mas tiempo:

`crunch 7 7 1980 > diccionario_opt.dic`

Una vez sacada la contraseña entramos en el ftp y dejamos nuestra marca en forma de un fichero de text con ascii-art y buscamos los planos del dron. Haciendo un listado del directorio revela un directorio `priv`. El nombre y el hecho de que no tenemos permisos para entrar deja claro que en este directorio se esconde algo. Desde el navegador también podemos sacar un listado de los ficheros subidos al ftp y entre ellos se encuentra el directorio `privado` que contiene un zip protegido con contraseña y con el *sospechoso* nombre de **PrototipoPrivado6.zip**:

`9/12/2015  9:50 AM        63493 PrototipoPrivado6.zip`

La información que hemos sacado anteriormente sobre las contraseñas facilita el crackeo de dicho fichero. Usamos fcrackzip con un wordlist común para no complicarnos más:

`fcrackzip -v -D -u -p /usr/share/wordlists/rockyou.txt PrototipoPrivado6.zip`

Dentro del zip encontramos el prototipo super privado del dron:

![]({{ site.baseurl }}/forestryio/images/Prototipo Super Privado.jpg)

## HackLab #2

Los dos objetivos a cumplir son:

1. Acceder a una página privada y
2. Descifrar el contenido del archivo secreto

La página privada `http://labs.gf0s.com/R2cde2/login/` pide una contraseña y tiene no tiene pinta de usar ningún tipo de cifrado. La pista es una [captura de tráfico de red](http://labs.gf0s.com/R2cde2/captura_de_red_reto2.pcap).

![]({{ site.baseurl }}/forestryio/images/2016-08-01 13_38_38-401 - Unauthorized_ Access is denied due to invalid credentials-1.png)

Analizamos la captura con Wireshark (o NetworkMiner) y encontramos los paquetes correspondientes al login (Basic access authentication):

![]({{ site.baseurl }}/forestryio/images/2016-08-01 13_40_59-captura_de_red_reto2.pcap.png)

El fichero que encontramos es un texto con un volcado de contraseña:

`Administrator:B61CCDB6BE3458D0AAD3B435B51404EE:A7B9ECDD64AA492E449E0A619FD16E4B:::`

Podemos usar hash-identifier o hashid para identificar el tipo y usar algún servicio online (como [onlinehashcrack.com](http://onlinehashcrack.com) para descifrarlo (ya que seguramente se trata de alguna contraseña muy débil).

## HackLab #3

En Hacklab #3 nos retan a encontrar TOCHAMA - un *terrorista que tiene en su poder información confidencial que interntará vender al mejor postor´. La primera pista es un código QR. Guardamos la imagen y lo escaneamos bien [online](http://webqr.com) o bien usando [Zbar](http://zbar.sourceforge.net/download.html):

![]({{ site.baseurl }}/forestryio/images/QR-HL3-Pista1.png)

```

Bienvenido agente. Has logrado acceder al contenido de la primera pista.
Nuestras investigaciones indican que TOCHAMA fue visto navegando en
http://gf0s.com/2014/08/20/h3-pista2-2
El codigo de acceso a la pagina es 4UGtm1#-69 

```

Lo que nos lleva a una entrada protegida de un WordPress. Ahí tenemos una copia digital de su pase de abordar. Debemos analizarlo para identificar el país al que TOCHAMA ha viajado. Esta pista tiene truco y podemos sacar información (por ejemplo del código de barra) que se ha metido para despistarnos:

![]({{ site.baseurl }}/forestryio/images/2016-08-01 14_02_33-Camino incorrecto-1.png)

La información sobre el destino se encuentra en los datos exif del pase:

![]({{ site.baseurl }}/forestryio/images/ticket_metadata.png)

Se fue a la India!

Para entrar en el área restringida nos pide una contraseña. Echamos un vistazo al código fuente de la página y encontramos el JavaScript que se preocupa de la validación de la contraseña:

``` js 

eval(function(p,a,c,k,e,d){e=function(c){return(c<a?'':e(parseInt(c/a)))+((c=c%a)>35?String.fromCharCode(c+29):c.toString(36))};if(!''.replace(/^/,String)){while(c--){d[e(c)]=k[c]||e(c)}k=[function(e){return d[e]}];e=function(){return'\\w+'};c=1};while(c--){if(k[c]){p=p.replace(new RegExp('\\b'+e(c)+'\\b','g'),k[c])}}return p}('j i(){5 2=1;5 0=7(\'k l h n - 3 f -\',\' \');b(2<3){4(!0)9.8(-1);4(0.6()=="a"){c(\'g d!\');e.m(\'y.o\');w}2+=1;5 0=7(\'A v - u q, p r s.\',\'t\')}4(0.6()!="x"&2==3)9.8(-1);z" "}',37,37,'pass1||testV||if|var|toLowerCase|prompt|go|history|2657|while|alert|agente|window|intentos|Bienvenido|clave|passWord|function|Introduce|la|open|correcta|html|Intenta|incorrecta|de|nuevo|Password|Clave|denegado|break|password|37463|return|Acceso'.split('|'),0,{}))

```

Esta ofuscado para complicarnos la vida así que procedemos a limpiarlo (por ejemplo con jsbeautifier):

``` js

function passWord() {
    var testV = 1;
    var pass1 = prompt('Introduce la clave correcta - 3 intentos -', ' ');
    while (testV < 3) {
        if (!pass1) history.go(-1);
        if (pass1.toLowerCase() == "2657") {
            alert('Bienvenido agente!');
            window.open('37463.html');
            break
        }
        testV += 1;
        var pass1 = prompt('Acceso denegado - Clave incorrecta, Intenta de nuevo.', 'Password')
    }
    if (pass1.toLowerCase() != "password" & testV == 3) history.go(-1);
    return " "
}

```

Mucho mejor!

Lo siguiente seria descargar un fichero de mega (la *pequeña ayuda* es la llave de desencriptación). Es una captura de tráfico en la cual encontramos petición y respuesta de lo que parece ser una imagen jpg. En Wireshark seleccionamos, entramos en los detalles y seleccionamos `Media type: image/jpeg`. Con `CTRL+H` (ó "Export Packet bytes") exportamos la imagen como image.jpg:

![]({{ site.baseurl }}/forestryio/images/2016-08-02 20_01_04-Reto3_Acertijo4.pcap.png)

El ultimo paso es una página con un mensaje de error que no dice que *Este sitio solo acepta conexiones con el siguiente navegador: 
GF0S/1.0 (Windows NT 6.3; WOW64) AppleWebKit/537.36 (KHTML, like Incorporado) Security/537.36.*

Abrimos el WebInspector (`CTRL + Mayús + I` en caso de Chrome), pulsamos `F1` para entrar en las preferencias y dentro de "Devices" añadimos nuestra cadena de [User-Agent](https://en.wikipedia.org/wiki/User_agent):

![]({{ site.baseurl }}/forestryio/images/solved.png)

Recargamos y listo!

![](https://media.giphy.com/media/l0O7P6qdCa1AKJRAY/giphy.gif)

Siempre mola la mezcla de técnicas diferentes y un poco de historia alrededor de los retos. 

Gracias al equipo de [GF0S](http://labs.gf0s.com/) por currárselo y crear un CTF bastante accesible también para novatos!
