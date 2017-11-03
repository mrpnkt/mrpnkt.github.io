---
title: RATas y serpientes
date: 2017-11-03 00:00:00 +0000
layout: post
tags:
- homework
- physec
---
## Un viaje por las alcantarillas de la red

Como parte de los estudios de un curso en el cual nos pedían crear un crypter y un troyano, me he dado un paseo por las alcantarillas de la red. No soy programador y nunca me interesó demasiado el tema de la “administración remota sin consentimiento del cliente” (por llamarlo de alguna forma políticamente correcta).

El mundo del malware es una zona gris legalmente hablando y la información que consultaba hasta entonces enfoca el tema desde el lado de reversing y de análisis. Aunque no cuesta demasiado encontrar recursos de creación de cualquier tipo de bicho - de rootkits a ransomware, para alguien que no sepa programar, el primer problema que se planta es por dónde empezar.

En los foros underground hay un montón de tutoriales sobre cómo crear troyanos en Visual Basic, AutoIt, .net, Java, C ó Assembler y en cientos de hilos tanto activos como abandonados con discusiones sobre sus supuestos pros y contras. Hay hilos donde se ofrecen crypters y troyanos ya creados para su modificación y uso -  tanto libre como de pago por el servicio prestado.

Está claro que el punto en común que tienen todos los RATs es que deben ser funcionales e indetectables por los productos antivirus, pero hay muchos matices y preferencias, hay mercado y hay mentes libres, se juntan ignorantes y novatos, gente con ganas de aprender y gente capaz de crear auténticas maravillas de crimeware para los fines que crean convenientes. Un mundo de mercado, de crimen organizado, y de estados opresores, pero también de la anarquía, del aprendizaje y del desafío a la autoridad.

Recomiendo leer diferentes artículos de análisis de malware para aprender sobre los distintos mecanismos de evasión de antivirus, de propagación y de persistencia. Me encanta leer los artículos sobre malware que se [burlan](https://twitter.com/zerosum0x0/status/925486850399019009) de manera inteligente e incluso graciosa de los productos de las grandes empresas, pero…

**¿Qué mejor manera de aprender sobre algo que creándolo uno mismo?**

### Recursos

El tema es tan amplio que resulta imposible abarcar en un simple artículo, así que solo voy a dar unas pautas de por dónde empezar:

#### Para Leer

En el blog de Security by Default hay una serie de excelentes artículos de introducción:

* [Introducción a los 'crypters'](http://www.securitybydefault.com/2013/07/introduccion-los-crypters.html)

* [Funcionamiento de los 'crypters'](http://www.securitybydefault.com/2013/07/funcionamiento-de-los-crypters.html)

* [El 'Stub': la pieza clave del 'crypter'](http://www.securitybydefault.com/2013/08/el-stub-la-pieza-clave-del-crypter.html)

* [Modding 'Crypters': el arte de la evasión](http://www.securitybydefault.com/2013/08/modding-crypters-el-arte-de-la-evasion.html)

* ['CRYPTERS': practicando la técnica dsplit/Avfucker](http://www.securitybydefault.com/2013/09/crypters-practicando-la-tecnica.html)

…y más:

* [A study of RATs, de Veronica Valeros](https://www.veronicavaleros.com/blog/2017/10/27/a-study-of-rats)

* [Métodos de evasión de firmas antivirus](https://foro.elhacker.net/analisis_y_diseno_de_malware/metodos_de_evasion_de_firmas_antivirus_enelpc-t438285.0.html)

* [Practical Malware Analysis de Kris Kendall y Chad McMillan](https://www.blackhat.com/presentations/bh-dc-07/Kendall_McMillan/Presentation/bh-dc-07-Kendall_McMillan.pdf)

* [Los slides de “Breaking Antivirus Software”](http://joxeankoret.com/download/breaking_av_software_44con.pdf) de [Joxean Koret](https://www.twitter.com/matalaz)

* [Reverse Engineering Malware 101](https://securedorg.github.io/RE101/) de [@malwareunicorn](https://www.twitter.com/malwareunicorn)

* [El libro Practical Malware Analysis](https://www.nostarch.com/malware)

#### Para Ver

Recomiendo leer también los comentarios, ya que en los hilos se plantean algunas cuestiones interesantes.

Videos de ponencias que profundizan sobre la evasión de antivirus:

* [The Art of Evading Antivirus](https://www.youtube.com/watch?v=XdfF5swMvrg)

* [Basic Anti-Virus Bypass Techniques](https://www.youtube.com/watch?v=S8kCCeCEALE)

* [TROOPERS14 - Easy Ways To Bypass Anti-Virus Systems - Attila Marosi](https://www.youtube.com/watch?v=Sl1Sru3OwJ4)

* [ROPInjector: Using Return Oriented Programming For Polymorphism And Antivirus Evasion](https://www.youtube.com/watch?v=YYbI8hbhrSY)

* [Thead Not Found, Matando los Antivirus](https://www.youtube.com/watch?v=XpcJ9Jd4Ud0) ([Slides](https://speakerdeck.com/sheilaberta/threat-not-found-owasp-patagonia))

* [Los videos del canal MalwareAnalysisForHedgehogs](https://www.youtube.com/channel/UCVFXrUwuWxNlm6UNZtBLJ-A/playlists)

#### Comunidad

Algunos foros donde se tratan temas de malware/modding/programación.

* [Level23Hacktools](http://level23hacktools.com/forum/)

* [Indetectables](https://www.indetectables.net/forum.php)

* [Underc0de](https://underc0de.org/foro/)

* [hackxcrack](https://foro.hackxcrack.net)

* [Hackplayers](http://foro.hackplayers.com)

* [0x00sec.org](https://0x00sec.org/)

Y por supuesto hay cientos más… ¡encuéntralas! ¿Porque limitarse solo al castellano e inglés? Usa el traductor de Google. ¿Quizás aprender un poco de ruso es demasiado, pero portugués o italiano? Los canales de telegram son una buena fuente de recursos gratuitos y buenos consejos.

Conviene comportarse, leer las reglas de cada comunidad y evitar las preguntas tontas (_¡sí, existen las preguntas tontas!_). No preguntes antes de haber investigado tú mismo y haber usado la función de búsqueda. Quedarás mal antes todos.

#### Laboratorio de malware

Para investigar malware es conveniente crear un laboratorio de máquinas virtuales.

* [Using VMware for Malware Analysis](https://zeltser.com/vmware-malware-analysis/)

* [malboxes](https://www.fireeye.com/blog/threat-research/2017/07/flare-vm-the-windows-malware.html) - una herramienta para crear máquinas virtuales funcionales y seguras

* [FlareVM](https://www.fireeye.com/blog/threat-research/2017/07/flare-vm-the-windows-malware.html) – una distribución de seguridad basada en Windows, de acceso libre y de libre acceso, diseñada para la ingeniería inversa, análisis de malware e investigación forense

* [Building a Home Lab to Become a Malware Hunter - A Beginner’s Guide](https://www.alienvault.com/blogs/security-essentials/building-a-home-lab-to-become-a-malware-hunter-a-beginners-guide)

* El libro [“Building Virtual Machine Labs”](https://leanpub.com/avatar) de [@da_667](https://www.twitter.com/da_667)

* [Máquinas virtuales de Microsoft](https://developer.microsoft.com/en-us/microsoft-edge/tools/vms/)

* [Microsoft Windows and Office ISO Download Tool](https://www.heidoc.net/joomla/technology-science/microsoft/67-microsoft-windows-and-office-iso-download-tool)

#### Herramientas

Se puede perder mucho tiempo en la búsqueda de herramientas que a primera vista parecen imprescindibles. No lo son y casi siempre hay [una alternativa](https://www.alternativeto.net), pero dejo aquí unos recursos que al principio cuesta encontrar (un consejo: [apunta las contraseñas](https://keepassxc.org)) de los archivos comprimidos):

* [Megapack Modding Tools de fudmario](http://www.sniferl4bs.com/2014/02/conociendo-sobre-malware-vi-mega-pack.html)

* FLM Antinub (Encoder/Decoder Pack) `hxxp://www.connect-trojan DOT net/2016/01/flm-antinub-v1.0-tools-encoder-and-decoder.html` _(¡Cuidado con esta página!)

* [Microsoft Sysinternals Suite](https://docs.microsoft.com/en-us/sysinternals/downloads/sysinternals-suite)

* [Ronda OCX](https://indetectables.net/viewtopic.php?t=19419)

#### Apuntes

El mejor recurso sin embargo es la investigación propia, hacer apuntes sobre todo que uno vaya encontrando y la experimentación.

Suelo usar el software [CherryTree](http://www.giuspen.com/cherrytree/), para estructurar los apuntes y tenerlos a mano de una forma ordenada que facilita la búsqueda. Otra opción es [TiddlyWiki](http://tiddlywiki.com), una mezcla de cuaderno y wiki y cuaderno personal.

![CherryTree](https://i.imgur.com/Ps38AHu.png)

Recomiendo hacer copias de respaldo de los archivos descargados, snapshots de máquinas virtuales e incluso archivar las páginas web [antes de que desaparezcan](http://archive.is/).

#### Seguridad operacional

Ante todo – seguridad, porque **si juegas con fuego te puedes quemar**. Ten asumido que tarde o temprano [tú también “caerás”](https://decentsecurity.com/how-you-get-infected/). Ten preparado todo para este caso. Usa una máquina dedicada a la investigación de malware e intenta no mezclar tu vida privada con tu vida “profesional”. Ten cuidado [en los viajas](https://www.zdziarski.com/blog/?p=6918) y en lugares públicos.

Aprende lo básico sobre la Seguridad Operacional ([OPSEC – Introducción a la seguridad operacional](https://www.fwhibbit.es/opsec-introduccion-a-la-seguridad-operacional) y [OPSEC - Compartimentalización, identidades y dominios de seguridad](https://www.fwhibbit.es/opsec-compartimentalizacion-identidades-y-dominios-de-seguridad)).

Mantén [limpio, actualizado y seguro tu sistema operativo](https://decentsecurity.com/securing-your-computer/), usa [Sandboxie](https://www.sandboxie.com/) para ejecutables sospechosos, una [conexión segura](https://www.privacytools.io/#vpn) y [asegura tu red local](https://store.pfsense.org).

### Manos a la obra – El troyano

Después de echar un vistazo a troyanos programados en otros idiomas decidí quedarme con Python, contrario a Java, Visual Basic o C# me resulta más fácil comprender lo que hace cada línea de código. Además, no iba a perder el tiempo reinventando la rueda del concepto cliente servidor (que por cierto está [muy bien explicado aquí](https://www.fwhibbit.es/python-hacking-simple-cliente-servidor) y [también aquí](http://www.semecayounexploit.com/?sec=python&nota=19)) y me encontré con un excelente ejemplo en github:

![](https://i.imgur.com/9txG0bl.png)

[Ares - Python botnet and backdoor](https://github.com/sweetsoftware/Ares) de [sweetsoftware](https://github.com/sweetsoftware), modular e independiente de plataforma. Perfecto para destriparlo como ejemplo.

#### Preparación del laboratorio

Dos máquinas de Windows 10 totalmente actualizados; una para hacer de víctima, la otra para montar el malware y hacer de atacante. Es importante apagar la telemetría y el envío de muestras de defender tanto en los guests como en el host.

Lo siguiente es instalar Python ([este tutorial lo explica bastante bien](https://matthewhorne.me/how-to-install-python-and-pip-on-windows-10/), aunque en este caso usaré la versión 2.7), el [Microsoft Visual C++ Compiler para Python 2.7](http://www.microsoft.com/en-us/download/details.aspx?id=44266), las dependencias (requests, cherrypi, [pythoncom](https://superuser.com/questions/609447/how-to-install-the-win32com-python-library), [pyHook](https://stackoverflow.com/questions/32446146/import-pyhook-failed/35118829), [PIL](https://www.lfd.uci.edu/\~gohlke/pythonlibs/#pil)) y [pyinstaller](http://www.pyinstaller.org/).

#### Ares, el padre

Ares viene en dos partes:

1. **El servidor de Command and Control** (CnC), básicamente una interfaz web para administrar a los agentes (clientes, bots, troyanos, …cada uno lo llama como quiera)

2. **El “agente”** (agent.py), el programa que corre en un host comprometido y comunica con el CnC. [Mediante módulos](https://github.com/sweetsoftware/Ares/tree/master/agent/python/modules) se añaden funciones como ddos, persistencia, keylogging o capturas de pantalla.

Tanto para agente como para el servidor hay un fichero de configuración `agent/settings.py y server/conf/server.conf` donde se definen bot-ids, puertos y opciones de debugging entre otras.

El server se arranca con un python server.py desde la consola: