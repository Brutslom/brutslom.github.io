---
layout: post
title: MAQUINAS 'STARTING POINT' TIER 0 (HTB). EXPLOSION (ES)
categories:
- español
- laboratorios
tags:
- Hack The Box
- Punto de partida
- Maquinas
- Tier 0
img_path: "/assets/img/posts/20230321/"
image:
  path: portada.png
  alt: EXPLOSION
date: 2023-03-21 00:00 +0000
---
Hola a todos,

Hoy vamos a ver la quinta máquina del nivel "very easy" del apartado de los laboratorios "Starting Point" llamada [**Explosion**](https://app.hackthebox.com/starting-point), donde estaremos viendo una máquina Windows.

# Introducción

Una vez activada la máquina nos facilita la ip _10.129.243.76_ por lo que las situaciones donde se indique esta ip se deberá cambiar por la facilitada por la plataforma.

> Recuerda que las respuestas deberán estar en Inglés
{: .prompt-warning }

> En todas las preguntas aparecen unos asteriscos (*) y alguna letra, que facilita la resolución de la tarea.
{: .prompt-tip }

# Tareas

## Tarea 1

> ¿Qué significa el acrónimo de 3 letras RDP?

Si buscamos en internet el significado de este acrónimo nos encontramos con su valor **Remote Desktop Protocol**, se trata de un protocolo para acceder a un escritorio remoto.

## Tarea 2

> ¿Cuál es el acrónimo de 3 letras que se refiere a la interacción con el host a través de una interfaz de línea de comandos?

El acrónimo con el que se suele referir cuando interactuamos con la consola es **CLI**.


## Tarea 3

> ¿Qué se refiere a las interacciones con la interfaz gráfica de usuario?

El acrónimo que se utiliza cuando se usa la interfaz gráfica de usuario es **GUI**.

## Tarea 4

> ¿Cuál es el nombre de una antigua herramienta de acceso remoto que venía sin encriptación por defecto y que escucha en el puerto TCP 23?

Generalmente el servicio que corre bajo el puerto 23 es **telnet**, es un protocolo que permite el manejo de una máquina remota.

## Tarea 5

> ¿Cuál es el nombre del servicio que se ejecuta en el puerto 3389 TCP?

Si ejecutamos el comando:

```bash
nmap -sV -p3389 10.129.243.76
```

- -sV: Permite encontrar la versión del servicio.
- -p3389: Sirve para solo centrarse en el puerto indicado.

Una vez lanzado el comando vemos que el servicio que está corriendo bajo ese puerto es **ms-wbt-server**.

## Tarea 6

> ¿Cuál es el modificador utilizado para especificar la dirección IP del host de destino cuando se utiliza xfreerdp?

xfreerdp es una herramienta obsoleta pero si buscamos en su ayuda podemos ver que para seleccionar el host de destino podemos usar el parámetro: **/v:**


## Tarea 7

> ¿Qué nombre de usuario nos devuelve con éxito una proyección de escritorio con una contraseña en blanco?

Para saber que usuario puede acceder sin contraseña, podemos identificar a los nombre de usuario por defecto mas conocidos esto se puede consultar a través de la herramienta [**seclist**](https://github.com/danielmiessler/SecLists/blob/master/Usernames/top-usernames-shortlist.txt), donde nos encontramos con los siguientes usuarios:

- root
- admin
- test
- guest
- info
- adm
- mysql
- user
- administrator
- oracle
- ftp
- pi
- puppet
- ansible
- ec2-user
- vagrant
- azureuser

Si probamos la conexión con los diferentes usuarios podemos comprobar que con el usuario **administrator** podremos acceder.


# Root Flag

> En este apartado nos solicita que obtengamos la flag que se encuentra "oculta" en el sistema

Una vez hemos entrado en la máquina objetivo vemos un archivo llamado flag que contiene el siguiente texto **951fa96d7830c451b536be5a6be008a0**

___

Con esto concluimos la quinta máquina del apartado "starting-point" de HTB, nos vemos en el siguiente post con la máquina _Preignition_.

Saludos.

