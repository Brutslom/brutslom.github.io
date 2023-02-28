---
layout: post
title: MAQUINAS 'STARTING POINT' TIER 0 (HTB). FAWN (ES)
categories:
- español
- laboratorios
tags:
- Hack The Box
- Punto de partida
- Maquinas
- Tier 0
img_path: "/assets/img/posts/20230228/"
image:
  path: portada.png
  alt: FAWN
date: 2023-02-28 18:40 +0100
---
Hola a todos,

En esta ocasión vamos a estar resolviendo las tareas de la segunda máquina de los laboratorios "Starting Point" llamada [**Fawn**](https://app.hackthebox.com/starting-point)

# Introducción

Una vez activada la máquina nos facilita la ip _10.129.155.17_ por lo que las situaciones donde se indique esta ip se deberá cambiar por la facilitada por la plataforma.

> Recuerda que las respuestas deberán estar en Inglés
{: .prompt-warning }

> En todas las preguntas aparecen unos asteriscos (*) y alguna letra, que facilita la resolución de la tarea.
{: .prompt-tip }

## Tarea 1

> ¿Qué significa el acrónimo de 3 letras FTP?

Las siglas FTP significa **File Transfer Protocol**, es un protocolo que, como su nombre indica, permite la transferencia de archivos entre dispositivos.

# Tarea 2

> ¿En qué puerto suele escuchar el servicio FTP?

Generalmente, salvo que se haya modificado la configuración del servicio el protocolo FTP escucha por el puerto **21**.


# Tarea 3

> ¿Qué acrónimo se utiliza para la versión segura de FTP?

Por defecto, el protocolo FTP no ofrece "seguridad" y viajan los datos en texto plano, pero su versión segura, Secure File Transfer Protocol o **SFTP**, realiza una implementación donde codifica los datos que se intercambian.


# Tarea 4

> ¿Cuál es el comando que podemos utilizar para enviar una solicitud de eco ICMP para probar nuestra conexión con el objetivo?

Esto lo vimos en el anterior post, recordamos que la herramienta mas conocida para lanzar tramas ICMP es **ping**.

# Tarea 5

> Según tus escaneos, ¿qué versión de FTP se está ejecutando en el objetivo?

En esta tarea empezamos a interactuar con la máquina, y veremos la versión que está ejecutando lanzando la herramienta nmap:

```bash
 nmap -sV -p21 10.129.155.17
```

- -sV: Permite encontrar la versión del servicio.
- -p21: Sirve para solo centrarse en el puerto indicado.

Como se puede comprobar el servicio que se esta ejecutando es **vsftpd 3.0.3**

![nmap](nmap-1.png){: width="972" height="589" }
_nmap ftp_


# Tarea 6

> Según tus escaneos, ¿qué tipo de sistema operativo se está ejecutando en el objetivo?

Para buscar el sistema operativo de un equipo podemos usar nmap junto con su variable -O para obtener el sistema operativo pero en este caso el comando necesita permisos de administrador, esto lo conseguimos lanzando la herramienta con sudo. 

> Existen otras formas de obtener el sistema operativo con esta herramienta. [**Man Nmap**](https://nmap.org/book/man-os-detection.html)
{: .prompt-tip }


```bash
 sudo nmap -O 10.129.155.17
```

En este caso se puede comprobar que con el comando lanzado no es posible obtener el resultado esperado, por lo que debemos lanzar el comando con un poco de ayuda, intentando que nmap intente adivinar el sistema operativo.

```bash
 sudo nmap -O --osscan-guess 10.129.155.17
```

En este caso vemos que se obtienen varias opciones:

- Linux 5.0 (95%)
- Linux 5.0 - 5.4 (95%)
- Linux 5.4 (94%)
- HP P2000 G3 NAS device
- Linux 4.15 - 5.6
- Linux 5.3 - 5.4 (93%)
- Linux 2.6.32 (92%)
- Linux 5.0 - 5.3 (92%)
- Linux 5.1 (92%)
- Ubiquiti AirOS 5.5.9 (92%)

![nmap](nmap-2.png){: width="972" height="589" }
_nmap so_


En este caso vemos que lo casi 100% se trata de un sistema operativo Linux, sin embargo, si intentamos introducir el resultado vemos que nos da un error, sin embargo, nos dan una pista, la respuesta solo tiene 4 letras y como bien sabemos todos o casi todos los sistemas operativos Linux están basados en **unix**, que será la respuesta correcta.


# Tarea 7

> ¿Cuál es el comando que debemos ejecutar para mostrar el menú de ayuda del cliente "ftp"?

Para poder utilizar la herramienta `ftp` debemos instalarla en nuestro sistema, mediante el comando:

```bash
 sudo apt install ftp
```

> Dependerá de tu sistema operativo el método para la instalación de la herramienta.
{: .prompt-warning }

Como la mayoría de las herramientas se sigue un convenio no escrito en el que los comandos de ayuda de la herramienta se pueden lanzar mediante las siguientes variables de comando:

- -m
- -h
- -s
- help
- man

En este caso el comando que deberemos utilizar es  **ftp -h**


![ftp](ftp-1.png){: width="972" height="589" }
_ftp_


# Tarea 8

> ¿Cuál es el nombre de usuario que se utiliza en FTP cuando se quiere acceder sin tener una cuenta?

Si buscamos información sobre el servicio [**ftp**](https://www.uv.es/bombinp/ftp.html), podemos comprobar que existe un usuario que se crea de manera automática en el servicio cuando se instala en una máquina, mediante la configuración es posible anular la entrada de este usuario al servicio FTP, este usuario es **anonymous**


# Tarea 9

> ¿Cuál es el código de respuesta que obtenemos para el mensaje FTP 'Login successful'?

Si intentamos conectarnos como el usuario anonymous al servicio ofrecido por la máquina mediante el comando:

```bash
  ftp 10.129.155.17
```

En el usuario tendremos que poner _anonymous_ y la contraseña la podemos dejar vacía.

Como se puede comprobar antes de la cadena Login successful, vemos el código **230**.

![ftp](ftp-2.png){: width="972" height="589" }
_ftp_


# Tarea 10

> Hay un par de comandos que podemos utilizar para listar los archivos y directorios disponibles en el servidor FTP. Uno es dir. Cuál es el otro que es una forma común de listar archivos en un sistema Linux.

Es por todos bien conocido que el comando para listar archivos en Linux es **ls**. Podemos encontrar mas información en el este [**link**](https://www.freecodecamp.org/espanol/news/el-comando-linux-ls-como-listar-archivos-en-un-directorio-indicadores-de-opcion/)


# Tarea 11

> ¿Cuál es el comando utilizado para descargar el archivo que encontramos en el servidor FTP?

Si buscamos en la ayuda del comando ftp una vez conectado podemos ver los diferentes comandos:

![ftp](ftp-3.png){: width="972" height="589" }
_ftp_


Si buscamos información sobre que hace cada uno de los comandos ([**link**](https://www.serv-u.com/linux-ftp-server/commands)), podemos comprobar que para obtener un archivo debemos utilizar el comando **get**.


# Root Flag

> En este apartado nos solicita que obtengamos la flag que se encuentra "oculta" en el sistema

Para obtener la flag debemos buscarla en el sistema al que tenemos acceso.

Una vez conectados podemos listar el contenido de la carpeta al que tenemos acceso como usuarios anonimos, y ver que se encuentra un archivo llamado _flag.text_. Podemos obtenerlo llamando al comando _get_ y una vez descargado mostrarlo con el comando _cat_:

```bash
 get flag.txt
 exit
 cat flag.txt
```

Tras la ejecución de estos comandos se puede comprobar que la flag es: **035db21c881520061c53e0536e44f815**

![flag](flag.png){: width="972" height="589" }
_flag_


___

Con esto concluimos la segunda máquina del apartado "starting-point" de HTB, nos vemos en el siguiente post con la máquina _Dancing_.

Saludos.

