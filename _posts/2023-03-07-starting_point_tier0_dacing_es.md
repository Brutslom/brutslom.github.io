---
layout: post
title: MAQUINAS 'STARTING POINT' TIER 0 (HTB). DANCING (ES)
categories:
- español
- laboratorios
tags:
- Hack The Box
- Punto de partida
- Maquinas
- Tier 0
img_path: "/assets/img/posts/20230307/"
image:
  path: portada.png
  alt: DANCING
date: 2023-03-07 19:43 +0100
---
Hola a todos,

Hoy vamos a ver la tercera máquina del nivel "very easy" del apartado de los laboratorios "Starting Point" llamada [**Dancing**](https://app.hackthebox.com/starting-point), en este caso se trata de una máquina Windows.

# Introducción

Una vez activada la máquina nos facilita la ip _10.129.240.148_ por lo que las situaciones donde se indique esta ip se deberá cambiar por la facilitada por la plataforma.

> Recuerda que las respuestas deberán estar en Inglés
{: .prompt-warning }

> En todas las preguntas aparecen unos asteriscos (*) y alguna letra, que facilita la resolución de la tarea.
{: .prompt-tip }

# Tareas

## Tarea 1

> ¿Qué significa el acrónimo de 3 letras SMB?

Las siglas SMB significan **Server Message Block**, es un protocolo que permite la compartición de recursos entre diferentes equipos que componen una red.

## Tarea 2

> ¿En qué puerto funciona SMB?

Generalmente, salvo que se haya modificado la configuración del servicio el protocolo SMB escucha por el puerto **445**.


## Tarea 3

> ¿Cuál es el nombre del servicio para el puerto 445 que apareció en nuestro escaneo Nmap?

Si realizamos un escaneo rápido sobre el equipo facilitado:

```bash
 nmap -sV -p445 10.129.240.148
```
- -sV: Permite encontrar la versión del servicio.
- -p445: Sirve para solo centrarse en el puerto indicado.

Como se puede comprobar el servicio que se esta ejecutando es **microsoft-ds**

![nmap](nmap-1.png){: width="972" height="589" }
_nmap ftp_

## Tarea 4

> ¿Cuál es la "bandera" o "interruptor" que podemos utilizar con la herramienta SMB para "listar" el contenido del recurso compartido?

Alguna de las herramientas que incluye el so Parrot para la conexión del protocolo es _smbclient_, si listamos su ayuda con la "flag" --help vemos que la manera de listar el contenido que ofrece un equipo es mediante el comando smbclient **-L** ip.

## Tarea 5

> ¿Cuántos recursos compartidos hay en Dancing?

Si ejecutamos el comando anterior sobre el equipo proporcionado podemos ver que tenemos un total de **4** directorios/archivos.

```bash
 smbclient -L 10.129.240.148
```

![smbclient](smbclient-1.png){: width="972" height="589" }
_smbclient -L_


## Tarea 6

> ¿Cuál es el nombre del recurso compartido al que podemos acceder con una contraseña en blanco?

Si intentamos acceder a cualquier recurso mediante el comando:

```bash
 smbclient smbclient \\\\10.129.240.148\\{Carpeta} -U=anon%pass
```
- Se debe indicar una \ para escapar el resto de barras, para la primera, ingresamos una \ más, por cada una de las que necesitamos (2) y en el segundo apartado, antes de la carpeta, debemos ingresar una mas de la que necesitamos (1).
- -U se utiliza para introducir un usuario y contraseña, en este caso ha sido aleatorio, si no se introduce el parámetro, se pedirá una contraseña y puede ser en blanco.

![smbclient](smbclient-2.png){: width="972" height="589" }
_smbclient \\IP\Carpeta -U_

Como se puede comprobar, es posible acceder a IPC$ o a **Workshares**, esta segunda es la que encaja con la respuesta buscada.


## Tarea 7

> ¿Cuál es el comando que podemos utilizar en el intérprete de comandos SMB para descargar los archivos que encontremos?

Una vez hemos accedido a la carpeta podemos buscar el comando para la descarga mediante el comando help.

![smbclient](smbclient-3.png){: width="972" height="589" }
_smbclient help_

Como se puede comprobar el comando que mas se asemeja a lo que buscamos es **get** que obtendrá un recurso de la máquina donde estamos conectados.


# Root Flag

> En este apartado nos solicita que obtengamos la flag que se encuentra "oculta" en el sistema

Para obtener la flag debemos buscarla en el sistema al que tenemos acceso.

Una vez conectados por el protocolo SMB podemos listar el contenido de la carpeta al que tenemos acceso mediante el comando ls y ver que tenemos acceso a dos carpetas mas:

- Amy.J
- James.P

Si accedemos a la primera carpeta nos encontramos con un archivo llamado  _worknotes.txt_ si lo descargamos no vemos nada de interés, si volvemos a la carpeta raíz y entramos en la carpeta James.P vemos un archivo llamado _flag.text_. Podemos obtenerlo llamando al comando _get_ y una vez descargado salimos y lo mostramos con el comando _cat_:

```bash
 smbclient \\\\10.129.240.148\\WorkShares -U=anon%pass
 ls
 cd Amy.J
 ls
 get worknotes.txt
 exit
 cat worknotes.txt
 smbclient \\\\10.129.240.148\\WorkShares -U=anon%pass
 cd James.P
 ls
 get flag.txt
 exit
 cat flag.txt
```

Tras la ejecución de estos comandos se puede comprobar que la flag es: **5f61c10dffbc77a704d76016a22f1664**

![flag](flag.png){: width="972" height="589" }
_flag_


___

Con esto concluimos la tercera máquina del apartado "starting-point" de HTB, nos vemos en el siguiente post con la máquina _Redeemer_.

Saludos.

