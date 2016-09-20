---
title: "#moocHackingMU - Semana 1 - Tarea 1"
date: '2016-09-19 08:41:00'
layout: post
---
## TAREA 1: Herramientas básicas para obtener información de servidores externos

La primera semana del [MOOC de Seguridad-2016 Hacking ético (2ª ed.)](https://mooc.mondragon.edu/courses/course-v1:INFORMATICA+Seguridad-2016+Hacking-etico/about) nos introduce a las herramientas básicas de Footprinting y Reconocimiento.

### Ping

> Como programa, ping es una utilidad diagnóstica en redes de computadoras que comprueba el estado de la comunicación del host local con uno o varios equipos remotos de una red IP por medio del envío de paquetes ICMP de solicitud (ICMP Echo Request) y de respuesta (ICMP Echo Reply). Mediante esta utilidad puede diagnosticarse el estado, velocidad y calidad de una red determinada. *(Wikipedia)*

Usamos la herramienta ping para obtener la dirección IP del servidor de prueba:

```

c:\> ping www.euskalert.net

Haciendo ping a www.euskalert.net [193.146.78.12] con 32 bytes de datos:
Tiempo de espera agotado para esta solicitud.

```

como no responde al ping intentamos con otro ejemplo:

```

c:\> ping pruebas.euskalert.net

Haciendo ping a pruebas.euskalert.net [193.146.78.38] con 32 bytes de datos:
Respuesta desde 193.146.78.38: bytes=32 tiempo=185ms TTL=54
Respuesta desde 193.146.78.38: bytes=32 tiempo=220ms TTL=54

Estadísticas de ping para 193.146.78.38:
    Paquetes: enviados = 2, recibidos = 2, perdidos = 0
    (0% perdidos),
Tiempos aproximados de ida y vuelta en milisegundos:
    Mínimo = 185ms, Máximo = 220ms, Media = 202ms
Control-C
^C

```

### Whois

> WHOIS es un protocolo TCP basado en petición/respuesta que se utiliza para efectuar consultas en una base de datos que permite determinar el propietario de un nombre de dominio o una dirección IP en Internet. *(Wikipedia)*

Para obtener la información de una base de datos WHOIS podemos usar la herramienta de la línea de comandos...

``` bash

$ whois www.euskalert.net

Connecting to NET.whois-servers.net...
Connecting to whois.interdomain.net...

Domain ID:
Registrar WHOIS Server: whois.interdomain.net
Registrar URL: http://www.acens.com/
Updated Date: 2015-10-07T08:15:23Z
Creation Date: 2006-10-31T00:56:37Z
Registrar Registration Expiration Date: 2016-10-31T11:56:37Z
Registrar: acens Technologies, S.L.U.
Registrar IANA ID: 140
Registrar Abuse Contact Email: abuse@acens.com
Registrar Abuse Contact Phone:+34.911418583
Domain Status: ok http://www.icann.org/epp#ok
Registry Registrant ID:
Registrant Name: Mondragon Goi Eskola Politeknikoa, J.M.A., S.Coop
Registrant Organization:
Registrant Street: Loramendi 4
Registrant City: Arrasate
Registrant State/Province: Gipuzkoa
Registrant Postal Code: 20500
Registrant Country: ES
Registrant Phone: 943794700
Registrant Fax:
Registrant Email: amanterola@eps.mondragon.edu
Registry Admin ID:
Admin Name: Mondragon Goi Eskola Politeknikoa, J.M.A., S.Coop
Admin Organization: Mondragon Goi Eskola Politeknikoa, J.M.A., S.Coop
Admin Street: Loramendi,4
Admin City: Arrasate
Admin State/Province: GIPUZKOA
Admin Postal Code: 20500
Admin Country: ES
Admin Phone: +34.943794700
Admin Fax:
Admin Email: sistemak@eps.mondragon.edu
Registry Tech ID:
Tech Name: RESPONSABLE DE DNS
Tech Organization: RESPONSABLE DE DNS
Tech Street: JULIAN CAMARILLO 6
Tech City: MADRID
Tech State/Province: MADRID
Tech Postal Code: 28013
Tech Country: ES
Tech Phone: +34.913752300
Tech Fax:
Tech Email: dns_admin@corp.terra.es
Name Server: ns1.mondragon.edu
Name Server: ns2.mondragon.edu
URL of the ICANN WHOIS Data Problem Reporting System: http://wdprs.internic.net/
>>> Last update of WHOIS database:2015-10-07T08:15:23Z<<<
For more information on Whois status codes, please visit
https://www.icann.org/resources/pages/epp-status-codes-2014-06-16-en

```

 o usando las diversas herramientas online:

![]({{ site.baseurl }}/forestryio/images/whois_1-1.png)

Una herramienta para la línea de comandos en Windows:
https://download.sysinternals.com/files/WhoIs.zip

### nmap

> Nmap es un programa de código abierto que sirve para efectuar rastreo de puertos [...] Se usa para evaluar la seguridad de sistemas informáticos, así como para descubrir servicios o servidores en una red informática, para ello Nmap envía unos paquetes definidos a otros equipos y analiza sus respuestas. Este software posee varias funciones para sondear redes de computadores, incluyendo detección de equipos, servicios y sistemas operativos. *(Wikipedia)*

Usamos nmap con [algunas opciónes](https://hackertarget.com/nmap-cheatsheet-a-quick-reference-guide/) contra el objetivo:

```bash

nmap -Pn -f -sS -sV -p80,21,443 www.euskalert.net

Starting Nmap 7.25BETA2 ( https://nmap.org ) at 2016-09-20 11:11 CEST
Nmap scan report for www.euskalert.net (193.146.78.12)
Host is up (0.21s latency).
PORT    STATE    SERVICE VERSION
21/tcp  filtered ftp
80/tcp  open     http    Apache httpd 2.4.7 ((Ubuntu))
443/tcp closed   https

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 19.73 seconds

```