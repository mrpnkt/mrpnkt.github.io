---
title: “Unhackable” – El orgullo precede a la caída
date: '2018-08-22 00:00:00'
categories: []
layout: post
slug: unhackable-el-orgullo-precede-a-la-caida
tags:
- bitfi
- hacking
- seguridad
---

_"De todos los métodos elaborados y sofisticados de hoy en día para hacer las billeteras seguras y fáciles de usar, seguramente ninguno es tan épico como el de la nueva billetera Bitfi. Varios de mis competidores han sido pioneros en métodos innovadores para proteger las claves privadas, pero Bitfi hizo todo lo posible para garantizar que la clave privada nunca pueda obtenerse por medios ilícitos. Ninguna otra billetera de hardware ha sido construida hasta este nivel de sofisticación."_

Esta fue la declaración de John McAfee en el lanzamiento de la billetera física (hardware wallet) Bitfi. Según la compañía Bitfi es “una nueva billetera de hardware totalmente inhackeable con tecnología de nueva generación diseñada para impulsar la adopción masiva de activos digitales”.

![]({{ site.baseurl }}/forestryio/images/2018-08-25 13_39_09-JKwN8lf.jpg (488×315).png)

**¿Qué es un Hardware Wallet?**

Existen diferentes modos de guardar o almacenar criptomonedas: Los [Hot wallets](https://en.bitcoin.it/wiki/Hot_wallet) son billeteras que pueden instalarse en diferentes dispositivos, ya sean smartphone u ordenadores, así como también pueden alojarse en sitios web de terceros; los [Paper wallets](https://en.bitcoin.it/wiki/Paper_wallet) son documentos que contienen todos los datos necesarios para generar cualquier número de claves privadas de Bitcoin, formando un monedero de claves; y los [Hardware Wallets](https://en.bitcoin.it/wiki/Hardware_wallet), dispositivos que se mantienen desconectados de Internet, y cuyo propósito es almacenar las claves privadas necesarias para poder [operar con Bitcoins](https://derten.com/soluciones-ti/bitcoin-color-cristal/) u otras criptomonedas.

La función de un monedero virtual offline es mantener fuera de línea las claves privadas necesarias para firmar las transacciones del usuario. En el momento de la operación conectamos el dispositivo con el smartphone o la computadora y es entonces cuando el software del fabricante envía la transacción al dispositivo externo para que sea firmada y devuelta. De esta forma las claves permanecen seguras en el dispositivo y la operación es considerada segura.

En lugar de usar una semilla mnemónica de 24 palabras como la mayoría de las billeteras, la billetera Bitfi usa una frase secreta única, generada por la CPU, que existe el tiempo suficiente para aprobar la transacción. Los fabricantes afirman que esta frase creada por el usuario es "_imposible de adivinar por los demás, pero es fácil de memorizar para el titular de la billetera_". Bitfi Wallet permite a los titulares de billeteras almacenar una cantidad ilimitada de fondos, utilizando un “_algoritmo patentado_” y de “_código abierto_” que calcula la clave privada a partir de la frase secreta única del usuario. Según el fabricante “_la clave privada solo existe por una fracción de segundo, el tiempo suficiente para aprobar la transacción y nunca se almacena en ninguna parte_”.

### La (in)seguridad de billeteras físicas

La elección de una billetera digital es una decisión fundamental para los que entran en el mundo de las criptomonedas y la seguridad es un aspecto determinante en el momento de elegir una buena billetera digital.

Ledger y Trezor son algunas de las marcas más conocidas del mercado y ambos han tenido graves vulnerabilidades: Saleem Rashid, un programador de 15 años descubrió un [fallo en la popular billetera de hardware Ledger](https://techcrunch.com/2018/03/21/a-15-year-old-hacked-the-secure-ledger-crypto-wallet/) que permitía obtener PINs secretos antes o después de enviar el dispositivo. Los agujeros, que Rashid [describe en su blog](https://saleemrashid.com/2018/03/20/breaking-ledger-security-model/), permitieron tanto un "ataque de la cadena de suministro" - es decir, un ataque que podría comprometer el dispositivo antes de que se enviara al cliente- como otro ataque que podría permitir el robo de claves privadas después de que el dispositivo fuese iniciado.

En 2015, Jochen Hoenicke [pudo extraer la clave privada](https://jochen-hoenicke.de/trezor-power-analysis/) de un TREZOR utilizando una técnica de análisis de energía. En la Defcon 25, Josh Datko y Chris Quartier [revelaron vulnerabilidades](https://www.youtube.com/watch?v=hAtoRrxFBWs) en el chip de STMicroelectronics. [Aquí explican](https://medium.com/@Zero404Cool/trezor-security-glitches-reveal-your-private-keys-761eeab03ff8) cómo no solo extraen el seed (semilla) de 24 palabras sino también el código PIN y la etiqueta del dispositivo. Con esta información se podría hacer una copia exacta del dispositivo.

Investigando sobre estos ataques y las vulnerabilidades graves en dispositivos de diferentes marcas, rápidamente nos damos cuenta de que las carteras de hardware no son tan seguras como se podría pensar en un primer momento, más si cabe teniendo en cuenta que su propósito es mejorar la seguridad. Más sorprendente resulta la arrogancia mostrada por los representantes de Bitfi ante las críticas y advertencias de la comunidad hacker.

### Las afirmaciones de Bitfi

_“A diferencia de otras billeteras, **la billetera Bitfi no se puede alterar**. Si alguna vez se pierde, se la roban, se desmonta y se analiza de manera forense, **no se pueden recuperar las claves privadas**, lo que hace que la billetera sea segura para ser comprada a cualquier persona dentro de la red de distribuidores de distribución autorizados. Debido a que el algoritmo Bitfi es **completamente de código abierto**, los usuarios pueden obtener fácilmente sus claves privadas **sin depender de terceros, incluido Bitfi**. Además, en lugar de tener que acceder a cada moneda desde carpetas individuales o billeteras múltiples, los usuarios pueden ver y controlar todas sus monedas y activos digitales en un solo lugar a través del tablero de Bitfi.”_

![]({{ site.baseurl }}/forestryio/images/2018-08-25 13_08_48-John McAfee on Twitter_ _For all you naysayers who claim that “nothing is unhack.png)

https://twitter.com/officialmcafee/status/1021805449681817600

En dos ocasiones McAfee y la empresa de Bitfi retaron a investigadores de todo el mundo ofreciendo bounties (recompensas para aquellas personas que detectaran fallos graves de seguridad). En [un tweet McAfee](https://twitter.com/officialmcafee/status/1021805449681817600) ofreció $100.000 para aquel que consiguiese hackear el dispositivo. [Tardaron una semana en conseguir acceso completo al sistema](https://twitter.com/OverSoftNL/status/1024684201575108615) (root access).

![]({{ site.baseurl }}/forestryio/images/2018-08-25 13_14_24-John McAfee on Twitter_ _The $100,000 bounty to anyone who can hack the https___.png)

https://twitter.com/officialmcafee/status/1022891967175376897

El fabricante Bitfi [aumentó la apuesta a $250.000](https://bitfi.com/bounty) en algo que parece ser más un truco de marketing que un bug bounty en condiciones. Aunque el alcance del reto es realmente pequeño y poco realista, siguieron con más hackeos:

![]({{ site.baseurl }}/forestryio/images/2018-08-25 13_15_38-OverSoft on Twitter_ _Short update without going into too much detail about BitF.png)

[https://twitter.com/OverSoftNL/status/1024684201575108615](https://twitter.com/OverSoftNL/status/1024684201575108615 "https://twitter.com/OverSoftNL/status/1024684201575108615")

* Ryan Castellucci, ingeniero de software y hacker de hardware, comentó que Bitfi parece ser un teléfono [Android desmantelado](https://twitter.com/ryancdotorg/status/1023674295073964032).
* @OverSoftNL [publicó la lista de directorios](https://twitter.com/OverSoftNL/status/1024198078193192961) del ROM en Pastebin. Aún más preocupante: [Reveló la supuesta inclusión de un conocido conjunto de programas maliciosos llamado Adups FOTA](https://thenextweb.com/hardfork/2018/07/31/bitfi-wallet-spyware-malware/), una plataforma de software espía que permite la transmisión de texto, llamadas, ubicaciones y datos de aplicaciones a un servidor en China cada 72 horas.
* Saleem Rashid, demostró que, a pesar de las afirmaciones de que no se puede alterar el estado del dispositivo, [Doom ](https://twitter.com/AbeSnowman/status/1027377982497861632)[funciona perfectamente](https://www.zdnet.com/article/challenge-accepted-15-year-old-hacking-prodigy-plays-doom-on-unhackable-bitfi/).

La afirmación de que algo es inhackeable llama la atención de cualquier profesional de seguridad. Nada es 100% seguro. No es algo binario que se puede dividir en: seguro/no seguro. Hay una gran área gris basado en el perfil de riesgo y el modelo de amenaza. Asegurar hardware correctamente no es una tarea fácil, porque la superficie de ataque es enorme.

Vamos a desmontar unas cuantas de las afirmaciones sobre el dispositvo:

### Afirmación 1: “Bitfi no se puede alterar”

![]({{ site.baseurl }}/forestryio/images/2018-08-25 13_18_45-Burstup on Twitter_ _@Bitfi6 I received the Bitfi device in a box that was rippe.png)

https://twitter.com/Burstup/status/1023921887947177984

En referencia a la afirmación de Bitfi sobre que su dispositivo es “inalterable” y no posee memoria, los investigadores de Pentest Partners encontraron un [chip con memoria no volátil](https://www.pentestpartners.com/security-blog/unhackable-bitfi-crypto-wallet-whats-all-the-fuss-about/). El firmware no está firmado y como bien [dice @cybergibbons](https://twitter.com/cybergibbons/status/1028407576134856704): _“No se debería poder instalar y ejecutar código arbitrario en un dispositivo seguro sin reemplazar el hardware.”_ Cosa que sí se puede hacer en un Bitfi.

<style>.embed-container { position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; } .embed-container iframe, .embed-container object, .embed-container embed { position: absolute; top: 0; left: 0; width: 100%; height: 100%; }</style><div class='embed-container'><iframe src='https://www.youtube.com/embed/bEzFm0WzSqI' frameborder='0' allowfullscreen></iframe></div>

### Afirmación 2 “No hay software en el dispositivo ni memoria.” (McAfee)

“_La memoria está en el blockchain, entonces no tenemos nada que hackear. No tenemos memoria para mirar; no hay nada en la billetera que alguien pueda usar para hackearlo,_” [afirma McAfee en un video](https://twitter.com/officialmcafee/status/1025011112364974080). Por supuesto, hay software en él. Está ejecutando un sistema operativo, tiene una CPU, etc., se trata de un móvil sin tarjeta SIM. De hecho, el dispositivo posee 8GB de memoria on-board y 1GB de RAM.

Saleem Rashid mostró que la clave y el salt se encuentran en la RAM y pueden ser recuperadas [incluso después de 17 horas](https://twitter.com/spudowiar/status/1029730287121518592):

![]({{ site.baseurl }}/forestryio/images/bitfiterminal.png)

### Afirmación 3 “Tu cerebro es tu monedero”

“_\[…\] Sí, puedes almacenar todo el dinero en tu cerebro, nunca tener que almacenar claves, no tienes que crear copias de seguridad en papel, nunca más tener que descargar o instalar nada \[…\]_” [afirman los creadores en un tweet](https://twitter.com/Bitfi6/status/1028412806570356742). Aunque dicen que Bitfi no es un brain wallet (lo que consiste en recordar una contraseña compleja, un truco que permite a cualquier persona tener millones de dólares en efectivo digital solo en su cerebro, sin necesidad de guardar ningún registro en una computadora), sí confirman que “_con Bitfi puedes almacenar TODOS tus activos con una frase. Entonces puedes tener 50 monedas diferentes, todas bajo una frase_”.

Sin embargo, resulta que la mente es un lugar sorprendentemente vulnerable para guardar la clave que permite acceder a los activos. Ryan Castellucci publicó una prueba de concepto llamado [Brainflayer ](https://github.com/ryancdotorg/brainflayer)que se aprovecha de esta vulnerabilidad. El problema, [dice Castellucci](https://www.wired.com/2015/07/brainflayer-password-cracker-steals-bitcoins-brain/), es que los humanos no eligen contraseñas fuertes y aleatorias tan bien como piensan que lo hacen. Y cualquier hacker puede adivinar pacientemente millones y millones de contraseñas, convirtiéndolas en claves privadas y probándolas en cada dirección de bitcoin en la cadena de bloques, es decir, el registro público de todas las ubicaciones de bitcoin. Incluso cuando un usuario de bitcoin cree que ha elegido una frase de contraseña suficientemente fuerte para su billetera cerebral, Castellucci dice que a menudo no puede hacer frente a los recursos de ladrones motivados por una recompensa instantánea en efectivo.

### Afirmación 4 “Código libre”

![]({{ site.baseurl }}/forestryio/images/2018-08-25 13_26_44-We are open source and all these - Twitter Search.png)

https://twitter.com/Bitfi6/status/1024525615666548737

En un primer momento Bitfi afirmaba que “_si alguna vez se pierde un dispositivo o incluso si Bitfi ya no está en el negocio, puede recuperar fácilmente sus fondos utilizando nuestro código de código abierto._” Aunque más tarde han aflojado sus declaraciones e incluso han [editado la declaración](https://twitter.com/omer_crypto/status/1023222622623084544) de su página web. Lo único “abierto” que hemos podido encontrar ha sido [este whitepaper](https://docs.wixstatic.com/ugd/9e18eb_6d75d420deeb46f0a31493fde61319df.pdf) en el cual explican algunos conceptos básicos del algoritmo usado.

### Afirmación 5 “La pantalla del dispositivo tiene un ángulo de visión estrecho para prevenir ataques de shouldersurfing”

Un fan de Bitfi [tuiteó una imagen de su Bitfi](https://twitter.com/cybergibbons/status/1028946735249453056) destacando un supuesto [ángulo de visión estrecho](https://twitter.com/Bitfi0/status/1030413366001643520) que lo protege de la vista de terceros. Desafortunadamente, la foto muestra tanto el salt como la frase de contraseña cuando la escribe, pero solo debería mostrar **********s:

![]({{ site.baseurl }}/forestryio/images/2018-08-25 13_29_50-Speculating_ What about bitfi's claim - Twitter Search.png)

[https://twitter.com/Bitfi0/status/1028935892138901504](https://twitter.com/Bitfi0/status/1028935892138901504 "https://twitter.com/Bitfi0/status/1028935892138901504")  

### La historia mejora con el tiempo

Para cualquiera que entiende algo de seguridad y sabe que es el [modelado de amenazas](https://www.owasp.org/index.php/Modelado_de_Amenazas) quedó bastante claro que Bitfi no es en ningún caso un dispositivo seguro. Quedó demostrado que es vulnerable a ataques de cadena de suministro, de shouldersurfing, de Man-in-the-middle, de Keylogging, y a [ataques tipo Evil Maid](https://twitter.com/spudowiar/status/1031301920303063045).

En definitiva: en cuestión de pocos días los investigadores han sido capaces de lo siguiente:

1. Rootear un dispositivo.
2. Interceptar toda comunicación SSL entre dispositivo y servidores.
3. Firmar una transacción Bitcoin bajo estas condiciones.
4. Interceptar clave y seed y mandarlo a otra máquina bajo estas condiciones.

![]({{ site.baseurl }}/forestryio/images/pwnie.jpg)

Además, @cybergibbons encontró [SSID](https://twitter.com/cybergibbons/status/1030796489738645504) y [contraseña de Wifi](https://twitter.com/cybergibbons/status/1030723142438871041) en uno de los dispositivos. Un [integrante del equipo de McAfee](https://mrpnkt.github.io/2018/unhackable-el-orgullo-precede-a-la-caida/) aparentemente [huyó tras haber robado fondos](https://twitter.com/officialmcafee/status/1031183858572701696). Bitfi tuvo que despedir a [uno de sus empleados/community managers](https://twitter.com/Bitfi6/status/1024742423791067137), porque insultó e [incluso llego a amenazar](https://twitter.com/WhiteHatScum/status/1026530080229445632) usando la cuenta oficial de la empresa. En los PWNIE Awards (ceremonia de premios anual que celebra los logros y fallas de los investigadores de seguridad y la comunidad de seguridad en la conferencia BlackHat) Bitfi [gano la categoría de “lamest vendor response”](https://pwnies.com/winners/).

La recompensa resulta ser un farol, diseñada para reclamar que no han sido hackeados porque la misma no se ha reclamado. En realidad, la recompensa cubre un solo ataque: enviar el dispositivo (que tiene una contraseña fuerte) a través de UPS (que lleva varios días) a un atacante. Esto no emula el mundo real, ni siquiera se acerca.

Quizás John McAfee debería haberse acordado de sus propias palabras “_No hay nada que no es hackeable_” ([Entrevista NBC Bay Area, 2016](https://www.nbcbayarea.com/on-air/as-seen-on/John-McAfee_-_There-Is-Nothing-That-Is-Unhackable__Bay-Area-369354151.html)) antes de haber iniciado esta contienda. Que la historia sirva de ejemplo. Nada es 100% seguro, TODO es hackeable.

![]({{ site.baseurl }}/forestryio/images/nothing.png)
