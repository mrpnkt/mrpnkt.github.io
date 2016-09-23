---
title: "#moocHackingMU - Semana 2 - Tarea 1"
date: '2016-09-21 00:00:00'
layout: post
tags:
- moocHackingMU
---
## TAREA 1: Capturando tráfico con Wireshark

### Primera parte: analizando un protocolo inseguro - Telnet.

> En esta tarea no vamos a realizar capturas en vivo de tráfico, sino que vamos a analizar trazas (capturas) ya realizadas con anterioridad y salvadas en archivos. En este caso, vamos a usar la traza telnet-raw.pcap, del repositorio de capturas disponible en Wireshark.

> Descárgate la traza en tu ordenador y ábrela con Wireshark. Esta traza ha capturado el tráfico de una sesión de Telnet entre el cliente y el servidor.

Descarga: [telnet-raw.pcap](https://wiki.wireshark.org/SampleCaptures?action=AttachFile&do=get&target=telnet-raw.pcap)

Analizando la captura de tráfico podemos observar que ocurre un intercambio de datos usando el protocolo [telnet](https://es.wikipedia.org/wiki/Telnet) entre la maquina 192.168.0.1 (Servidor) y la maquina 192.168.0.2 (Cliente). Según el timestamp de la captura la session empieza el 3 de diciembre 1999 a las 04:34:48 (CST) y termina el mismo día a las 4:35:42 (CST).

```
Server: 192.168.0.1 TCP 23 (1742 data bytes sent), Client: 192.168.0.2 (Linux) TCP 1254 (259 data bytes sent), Session start: 03/12/1999 4:34:48, Session end: 03/12/1999 4:35:42
```

El banner de telnet muestra como último login:

`Thu Dec  2 21:32:59 on ttyp1 from bam.zing.org`

Filtrando el tráfico telnet y haciendo click secundario sobre un paquete podemos usar la opción `Follow tcp stream` para visualizar el texto en plano de la session:

![](http://i.imgur.com/tg48got.png)

#### ¿Qué usuario y contraseña se ha utilizado para acceder al servidor de Telnet?

```
login: .."........"ffaakkee
.
Password:user
```

l/p: fake:user

#### ¿Qué sistema operativo corre en la máquina?

`OpenBSD 2.6-beta (OOF)`

#### ¿Qué comandos se ejecutan en esta sesión?

```bash

$ ls
$ ls -a
.         ..        .cshrc    .login    .mailrc   .profile  .rhost
$ /sbin/ping www.yahoo.com
$ exit

```

### Segunda parte: analizando SSL

> Para la realización de este ejercicio, descarga esta traza con tráfico SSL y abrela con Wireshark. SSL es un protocolo seguro que utilizan otros protocolos de aplicación como HTTP. Usa certificados digitales X.509 para asegurar la conexión.

Descarga: [x509-with-logo.cap](https://wiki.wireshark.org/SampleCaptures?action=AttachFile&do=get&target=x509-with-logo.cap)

#### ¿Puedes identificar en qué paquete de la trama el servidor envía el certificado?

Buscamos el paquete con la info `Hello, Certificate` mandado por la IP 65.54.179.198 a la IP 10.1.1.2. Usamos la opción `Decode As...` y seleccionamos `SSL`. Ahora podemos entrar a la vista detallada del paquete y extraer los `Packet bytes`:

![](http://i.imgur.com/4shWx26.png)

#### ¿El certificado va en claro o está cifrado? ¿Puedes ver, por ejemplo, qué autoridad ha emitido el certificado?

Lo guardamos como extract.der y podemos visualizar sus contenidos:

![](http://i.imgur.com/BSUYCtf.png)

Vemos que la autoridad emisora del certificado es Verisign.

#### ¿Qué asegura el certificado, la identidad del servidor o del cliente?

El servidor.

### Tercera parte: analizando SSH

> En la primera parte de este ejercicio hemos visto un protocolo no seguro, como Telnet. Una alternativa a usar Telnet a la hora de conectarnos a máquinas remotas es SSH, que realiza una negociación previa al intercambio de datos de usuario. A partir de esta negociación, el tráfico viaja cifrado. Descarga esta traza con tráfico SSH y abrela con Wireshark.

Descarga: [ssh.pcap](https://www.dropbox.com/s/eczzwy4i6hztv71/ssh.pcap?dl=0)

#### ¿Puedes ver a partir de qué paquete comienza el tráfico cifrado?

El primer paquete cifrado:

![](http://i.imgur.com/Uiz5mGt.png)

#### ¿Qué protocolos viajan cifrados, todos (IP, TCP...) o alguno en particular?

ssh crea un tunel por el cual viaja toda información. En este ejemplo no podemos ver que protocolos estan en uso.

#### ¿Es posible ver alguna información de usuario como contraseñas de acceso?

Los datos se transmiten cifrados así que no es posible ver la información de usuario o contraseñas.
