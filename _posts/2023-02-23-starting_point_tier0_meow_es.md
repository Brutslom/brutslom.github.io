---
layout: post
title: MAQUINAS 'STARTING POINT' TIER 0 (HTB). MEOW (ES)
categories:
- español
- laboratorios
tags:
- Hack The Box
- Punto de partida
- Maquinas
- Tier 0
img_path: "/assets/img/posts/20230223/"
image:
  path: portada.png
  alt: MEOW
date: 2023-02-23 19:33 +0100
---
Hola a todos,

En esta serie de post vamos a ir resolviendo las máquinas relacionadas con los laboratorios de "Starting Point" de Hack The Box empezando por la primera máquina llamada [**Meow**](https://app.hackthebox.com/starting-point)

# Introducción

 Vamos a obviar en todas las máquinas el proceso de conexión hacía la VPN de _HTB_ ya que se trata de un proceso sencillo el cual debe realizarse con el comando:

```bash
 sudo openvpn starting_point_username.ovpn
```

Una vez activada la máquina nos facilita la ip _10.129.238.113_ por lo que las situaciones donde se indique esta ip se deberá cambiar por la facilitada por la plataforma.

> Recuerda que las respuestas deberán estar en Inglés
{: .prompt-warning }

> En todas las preguntas aparecen unos asteriscos (*) y alguna letra, que facilita la resolución de la tarea.
{: .prompt-tip }

# Tareas 

## Tarea 1

> ¿Qué significa el acrónimo VM?

En este caso la respuesta es sencilla, **Virtual Machine**. Sino, una consulta rápida en [**google**](https://letmegooglethat.com/?q=%C2%BFQu%C3%A9+significa+el+acr%C3%B3nimo+VM%3F), lo solucionará.


## Tarea 2

> ¿Qué herramienta utilizamos para interactuar con el sistema operativo con el fin de emitir comandos a través de la línea de comandos, como el de iniciar nuestra conexión VPN? También se conoce como consola o shell.

La respuesta también es sencilla, **terminal**


## Tarea 3

> ¿Qué servicio usamos para formar nuestra conexión VPN en los laboratorios HTB?

Si revisamos la ayuda que nos proporciona [**hackthebox**](https://help.hackthebox.com/en/articles/5185687-introduction-to-lab-access), podemos comprobar que el servicio que utilizan es: **openVPN**


## Tarea 4

> ¿Cuál es el nombre abreviado de una 'interfaz de túnel' en el resultado de la secuencia de arranque de su VPN?

Para ver nuestras interfaz de red podemos usar el siguiente comando:

```bash
 ifconfig
```
Cuando revisamos nuestras interfaces de red podemos ver 3 interfaces:

- en02: que corresponde a la interfaz de red principal de mi portátil.
- enx00e04c6804f6: que corresponde a la interfaz de red de mi hub, donde tengo enchufado el cable de red y le facilita una ip gracias al DHCP.
- lo: que corresponde al loopback, para dirigir el tráfico hacia el mismo portátil.

![ifconfig](ifconfig-1.png){: width="972" height="589" }
_ifconfig_

Tras establecer la comunicación con la VPN de HTB mediante el comando:
```bash
 sudo openvpn starting_point_username.ovpn
```

Podemos ver que se crea una nueva interfaz de red:

![ifconfig](ifconfig-2.png){: width="972" height="589" }
_ifconfig_


- tun0, que es la que gestiona el acceso a la red de HTB.

Por lo que la respuesta en este caso es, **tun**

## Tarea 5

> ¿Qué herramienta usamos para probar nuestra conexión con el objetivo con una solicitud de eco ICMP?

La herramienta por defecto y mas conocida a nivel mundial para realizar solicitudes de trazas ICMP es: **ping**


## Tarea 6

> ¿Cuál es el nombre de la herramienta más común para encontrar puertos abiertos en un objetivo?

En este caso la mas conocida es nuestra amiga: **nmap**, que utilizaremos en todos o casi todos los objetivos con el fin de hallar los puertos abiertos


## Tarea 7

> ¿Qué servicio identificamos en el puerto 23/tcp durante nuestros escaneos?

Si ejecutamos nmap contra la máquina que nos ha facilitado podemos comprobar que servicio tenemos abierto:

```bash
 nmap -sV -p23 10.129.238.113
```

- -sV: Permite encontrar la versión del servicio.
- -p23: Sirve para solo centrarse en el puerto indicado.

> En esta búsqueda de servicios se podrá indicar todos los puertos que se deseen, aunque antes de lanzar esta búsqueda será mejor encontrar los puertos abiertos que se encuentran en la máquina, para centrarnos solo en estos y no demorar la herramienta.
{: .prompt-info }

Podemos ver que el servicio que se encuentra activo en ese puerto es:**telnet**

![nmap](nmap.png){: width="972" height="589" }
_nmap_


## Tarea 8

> ¿Qué nombre de usuario puede iniciar sesión en el objetivo a través de telnet con una contraseña en blanco?

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

Si probamos la conexión con los diferentes usuarios podemos comprobar que con el primer usuario **root** ya podemos acceder.

```bash
 telnet 10.129.238.113
```
![telnet](telnet-1.png){: width="972" height="589" }
_telnet_

> En esta ocasión hemos tenido "suerte" pero es posible lanzar un ataque de fuerza bruta con otras herramientas como [**Hydra**](https://github.com/vanhauser-thc/thc-hydra) o [**Patator**](https://github.com/lanjelot/patator), a través de diferentes protocolos
{: .prompt-info }

# Root Flag

> En este apartado nos solicita que obtengamos la flag que se encuentra "oculta" en el sistema

En este caso buscamos la flag que es un archivo que contiene una cadena de caracteres que debemos introducir y se suele encontrar oculta o se debe escalar privilegios para obtenerla.

```bash
 ls
 cat flag.txt
```

Tras la ejecución de estos comandos se puede comprobar que la flag es: **b40abdfe23665f766f9c61ecba8a4c19**
![telnet](telnet-2.png){: width="972" height="589" }
_telnet_

___

Esto sería todo para la primera máquina del "starting-point" de HTB, espero que os haya gustado y nos vemos en la próxima.

Saludos.

