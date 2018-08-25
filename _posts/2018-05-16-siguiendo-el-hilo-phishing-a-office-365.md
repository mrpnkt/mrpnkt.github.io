---
title: Siguiendo El Hilo Phishing A Office 365
date: '2018-05-16 00:00:00'
categories: []
layout: post
slug: siguiendo-el-hilo-phishing-a-office-365
tags:
- phishing
- o365
---

Últimamente, se han visto muchos intentos de ataques de ingeniería social a cuentas de Office 365. Tanto el Phishing como el Spearphishing, están más vivos que nunca.

De acuerdo con el informe [Fraud Beat de Easy Solutions](https://www.easysol.net/eng/press-releases/easy-solutions-offers-new-fraud-beat-2017), el **97% de las personas no puede reconocer con precisión un email de phishing** y cerca del 30% de estos emails son abiertos por sus destinatarios. Teniendo en cuenta estos indicadores, los creadores de estos ataques tienen serios incentivos para continuar con sus actividades delictivas.

El informe [Phishing Trends & Intelligence Report de Phishlabs](https://info.phishlabs.com/2017-phishing-trends-and-intelligence-report-pti) para el año 2017  alertó sobre que el correo electrónico y los servicios en línea (26% de los ataques) superaron a las instituciones financieras (21%) como el principal objetivo de phishing.

### ¿Por qué seguimos recibiendo correo basura aun teniendo todo tipo de mecanismos de seguridad (Antivirus, Cortafuegos, Filtros de SPAM)?

Los spammers y phishers crean malware y envían spam porque es rentable. Siempre están buscando nuevas formas de evitar los filtros de correo no deseado y enviar mensajes a las bandejas de entrada de los usuarios. Debido a la cantidad de spammers únicos en el mundo y la velocidad a la que crean contenido nuevo, el spam que ve en su bandeja de entrada hoy es nuevo. Es diferente de lo que era ayer, el día anterior o el día anterior. Se ve similar, y puede usar la misma técnica, pero no es el mismo mensaje. Es levemente (o enormemente) diferente y ha sido diseñado para evadir filtros.

Las campañas de spam varían en duración. Hay algunos que duran muchas horas, y algunos que duran unos minutos. Hay campañas que envían miles, cientos de miles o incluso millones de mensajes de correo no deseado en menos de 15 minutos.

Cuando veas un spam en tu bandeja de entrada, generalmente se debe a que es una campaña nueva de un spammer y todavía no se han recogido firmas para ella. Durante esta ventana, un spammer puede enviar algo de spam a través de las defensas de filtración.

![]({{ site.baseurl }}/forestryio/images/post-1.png)

Cuando veas un spam en tu bandeja de entrada, generalmente se debe a que es una campaña nueva de un spammer y todavía no se han recogido firmas para ella. Durante esta ventana, un spammer puede enviar algo de spam a través de las defensas de filtración.

#### Pretexto

Nuestras debilidades naturales como humamos, pueden ser explotadas en muchas ocasiones mucho más fácilmente que las de una red o un software. Bajo el uso de un escenario completamente inventado (pretexto) se puede [persuadir a una persona de cliquear un enlace, abrir un adjunto o entregar información](https://hipertextual.com/2015/03/que-es-la-ingenieria-social). Veamos algunos ejemplos de estos escenarios en campañas de phishing a cuentas de O365:

#### Docusign

En mayo 2017, **DocuSign**, un proveedor de servicios de firma electrónica, admitió que hubo una violación y se accedió a los correos electrónicos de los clientes. A continuación, un tercero envió mensajes a los correos electrónicos invitando a los usuarios a abrir un documento de Microsoft que contenía malware. Nuestro departamento de Ciberseguridad ha visto una campaña en cuyo contenido se pretende que el destinatario que ha recibido un documento de DocuSign, pinche en el enlace. Así, el usuario es llevado a una página en cual se pide introducir credenciales de Office 365.

![]({{ site.baseurl }}/forestryio/images/post-2.png)

El esquema conocido como **Afectación de Emails Corporativos (BEC)**.

![]({{ site.baseurl }}/forestryio/images/post3.png)

Reseteo de contraseña; cuenta suspendida, por encima de la cuota o actividad sospechosa.

Hay una gran variedad de intentos de Phishing bajo el pretexto de que haya alguna actividad sospechosa o que la cuenta de correo haya caducado (Account expired) o que la ésta esté por encima de la cuota otorgada (Mailbox full). Muchas veces, el correo llega en inglés y pretende parecer un correo enviado automáticamente o venir de Microsoft (por ejemplo, de un ficticio “Account Team”):

![]({{ site.baseurl }}/forestryio/images/post-4.png)

_"Actividad reciente"_ – Un correo sospechoso de _"Microsoft Teams"_

Igual que en el ejemplo anterior, el siguiente correo pretende venir de Microsoft (“en nombre de Microsoft Teams”). El alarmante contenido del correo indica que “una actividad” no detallada en la cuenta del usuario “_ha violado los términos de uso_”. Se amenaza con cerrar la cuenta en 24 horas si no se toman medidas. Jugando con la curiosidad y el miedo del usuario, un enlace que lee “_Check Recent Activities_”  incita a este a conocer más sobre lo que está ocurriendo:

![]({{ site.baseurl }}/forestryio/images/post-5.png)

Normalmente, una investigación se podría acabar aquí: se comprueban los remitentes y los enlaces en un sandbox virtual, se envían las muestras a Microsoft y se informa al usuario de lo ocurrido dándole unas pautas de como reconocer un phishing y como evitar ser víctima en el futuro. En este caso decidimos investigar un poco más allá.

Comprobamos el correo del remitente placings@daffodil.nocdirect\[.\]com, obviamente no tiene nada que ver con una cuenta oficial de Microsoft. La IP 69.73.181.16 aloja a más de 700 páginas web y es una verdadera jungla de SPAM, malware y páginas de phishing:

Curioso, cuanto menos.

![]({{ site.baseurl }}/forestryio/images/post-6.png)

![]({{ site.baseurl }}/forestryio/images/post-7-1024x455.png)

Curioso también ver la cantidad de registros de dominios en los últimos meses:

![]({{ site.baseurl }}/forestryio/images/post-8.png)

La página oficial de Network Transit Holdings no está operativa desde al menos el año 2015:

![]({{ site.baseurl }}/forestryio/images/post-9-1024x295.png)

Y la dirección del registrante de nocdirect.com lleva a una tienda UPS en Colorado Springs:

![]({{ site.baseurl }}/forestryio/images/post-10.png)

![]({{ site.baseurl }}/forestryio/images/post-11-1024x564.png)

No podemos hacer más que reportar el correo usado para enviar el mensaje y bloquearlo a nivel de Office 365. Así que pasamos a echar un vistazo al contenido.

### Saludos desde Brasil - La página del Phishing

Tomando las medidas de seguridad necesarias para tratar con este tipo de enlaces, empezamos a seguir el hilo dentro de una máquina virtual aislada de nuestro entorno de producción.

Confirmamos que el enlace de CHECK RECENT ACTIVITIES lleva a una página de inicio de sesión:

![]({{ site.baseurl }}/forestryio/images/post-12-1024x408.png)

Un análisis de la página nos revela más información como URL real, IP, enlaces entrantes y salientes:

![]({{ site.baseurl }}/forestryio/images/post-13-1024x456.png)

Podemos deducir de la estructura de la URL, que tenemos un fichero php (okli72kjxieujc2uf7ejm792.php) dentro de una instalación de WordPress (/wp-content/) a la cuál se pasan dos parámetros: **una cadena de texto y el correo de la víctima** (email=). La página serraheriacedros\[\]com\[.\]br pertenece a un taller de mecánica en Brasil y fue hackeada para alojar en ella el fichero malicioso.

![]({{ site.baseurl }}/forestryio/images/post-14-1024x439.png)

![]({{ site.baseurl }}/forestryio/images/post-15-1024x476.png)

Cambiando parámetros de la ruta, nos encontramos con un error y con el directorio donde se guarda el Phishing Kit. Estos se venden en la dark web y permiten crear páginas web falsas más convincentes, incluso a ciberdelincuentes con conocimientos básicos.

![]({{ site.baseurl }}/forestryio/images/post-16-1024x71.png)

![]({{ site.baseurl }}/forestryio/images/post-17.png)

Ciclo de vida de un Phishing:

![]({{ site.baseurl }}/forestryio/images/post-18.png)

[Las instalaciones de WordPress](https://www.wptemplate.com/tutorials/safety-and-security-of-wordpress-blog-infographic.html) son las victimas habituales de hackeos masivos y automatizados. Un estudio sobre [Phishing Kits](https://duo.com/assets/ebooks/phish-in-a-barrel.pdf) realizado por Duo Security destaca que tres de las 10 rutas principales en el conjunto de datos de los investigadores, indican que los phishing kits están alojados en una instancia comprometida de WordPress:

![]({{ site.baseurl }}/forestryio/images/post-19.jpg)

Es probable que el propietario del sitio web no esté al tanto de cómo se está abusando de su site y de él. Esta es una tendencia muy común en estos días. Los atacantes tienen un arsenal de opciones una vez que obtienen acceso a su sitio web.

Para ilustrar el impacto y la correlación entre los ataques de phishing y los ataques de malware más tradicionales, podemos echar un rápido vistazo al [informe de transparencia de Google](https://transparencyreport.google.com/safe-browsing/overview):

![]({{ site.baseurl }}/forestryio/images/post-20.png)

Teniendo indicadores como nombres de ficheros usados y el correo dentro del código fuente, podemos encontrar el supuesto autor de nuestro kit. Tiene presencia en varias redes sociales y no esconde sus motivaciones. En Pastebin aloja código de puertas traseras, en YouTube cuelga videotutoriales:

![]({{ site.baseurl }}/forestryio/images/post-21.png)

![]({{ site.baseurl }}/forestryio/images/post-22.png)

![]({{ site.baseurl }}/forestryio/images/post-23-1024x581.png)

Aunque quedan pistas que podríamos seguir, preferimos ser eficientes en vez de cazar fantasmas y nos limitamos a reportar tanto el phishing como los actores y los métodos usados.

### Como reportar un intento de phishing:

Para frenar intentos de phishing, es imprescindible informar de ello. Lo más seguro es un reenvío de cualquier correo sospechoso al departamento de ciberseguridad o TI de la propia empresa. Ellos se encargarán de notificar a todos agentes involucrados e implementar los bloqueos necesarios a nivel de red. También se puede informar directamente a [Microsoft ](https://www.microsoft.com/en-us/wdsi/support/report-unsafe-site)o [Google](https://www.google.com/safebrowsing/report_phish/). Lo ideal sería que en cuestión de horas, la página comprometida fuese limpiada por su webmaster o la cuenta suspendida por la empresa del hosting:

![]({{ site.baseurl }}/forestryio/images/post-24.png)
