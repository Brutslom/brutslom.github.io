---
layout: post
title: MAQUINAS 'STARTING POINT' TIER 0 (HTB). REDEEMER (ES)
categories:
- español
- laboratorios
tags:
- Hack The Box
- Punto de partida
- Maquinas
- Tier 0
img_path: "/assets/img/posts/20230314/"
image:
  path: portada.png
  alt: REDEEMER
date: 2023-03-14 00:00 +0000
---
Hola a todos,

Hoy vamos a ver la cuarta máquina del nivel "very easy" del apartado de los laboratorios "Starting Point" llamada [**Redeemer**](https://app.hackthebox.com/starting-point) y volvemos a una máquina Linux.

# Introducción

Una vez activada la máquina nos facilita la ip _10.129.8.75_ por lo que las situaciones donde se indique esta ip se deberá cambiar por la facilitada por la plataforma.

> Recuerda que las respuestas deberán estar en Inglés
{: .prompt-warning }

> En todas las preguntas aparecen unos asteriscos (*) y alguna letra, que facilita la resolución de la tarea.
{: .prompt-tip }

# Tareas

## Tarea 1

> ¿Qué puerto TCP está abierto en la máquina?

Para averiguar los puertos abiertos debemos usar la herramienta nmap con los siguientes comandos:

```bash
sudo nmap -p- --open 10.129.8.75
```

- -p-: Indicamos que debe escanear todos los puertos, los 65535
- --open: Indicamos que solo queremos obtener los puertos abiertos.

Una vez lanzado el comando nos devuelve un único puerto abierto el **6379**.

## Tarea 2

> ¿Qué servicio se está ejecutando en el puerto abierto en la máquina?

Con el comando anterior, vemos que también nos ha facilitado el servicio que corre en el puerto, en este caso es **redis**.


## Tarea 3

> ¿Qué tipo de base de datos es Redis? Elija entre las siguientes opciones: (i) Base de datos en memoria, (ii) Base de datos tradicional

Aunque la pista es bastante clara, si realizamos una búsqueda en Google vemos que redes es una **In-memory Database**, este tipo de BBDD permite un análisis de los datos mucho mas rápido en comparación con la BBDD tradicional, gracias a que sus datos se almacenan en la memoria principal RAM. 

## Tarea 4

> ¿Qué utilidad de línea de comandos se utiliza para interactuar con el servidor Redis? Introduzca el nombre del programa que introduciría en el terminal sin ningún argumento.

Para trabajar con BBDD de redis, desde la línea de comandos, existe una herramienta que se llama **redis-cli**

## Tarea 5

> ¿Qué bandera se utiliza con la utilidad de línea de comandos Redis para especificar el nombre de host?

Si buscamos en la ayuda de redis-cli mediante el comando:

```bash
 redis-cli -help
```

Podemos comprobar que el parámetro que se utiliza es **-h**


## Tarea 6

> Una vez conectado a un servidor Redis, ¿qué comando se utiliza para obtener la información y las estadísticas sobre el servidor Redis?

Una vez conectado con el comando:

```bash
 redis-cli -h 10.129.8.75
```

con el comando _help_ y el tabulador podemos ir viendo todos los posibles comandos y nos llama la atención uno en concreto que se llama **info** si le damos a enter vemos que en su resumen nos facilita información y estadísticas sobre el servidor


## Tarea 7

> ¿Cuál es la versión del servidor Redis que se utiliza en la máquina de destino?

Si ejecutamos el comando visto en la pregunta anterior, vemos que la primera línea nos da la versión: **5.0.7**

## Tarea 8

> ¿Qué comando se utiliza para seleccionar la base de datos deseada en Redis?

Al igual que anteriormente con el parámetro help, vemos que tenemos un comando llamado **select** cuya información nos indica que nos permite seleccionar la BBDD de la conexión a través de su indice.


## Tarea 9

> ¿Cuántas claves hay dentro de la base de datos con índice 0?

Elegimos la BBDD con el comando

```bash
 select 0
```
Para obtener el número de clave-valor, debemos indicar el comando dbsize y nos facilita que tenemos un total de **4** claves.

## Tarea 10

> ¿Qué comando se utiliza para obtener todas las claves de una base de datos?

Si buscamos en la con la ayuda de nuevo vemos que tenemos un comando llamado _keys_, pero necesitamos un patrón por lo que si pensamos en patrones de regex podemos valorar que para seleccionar todas las keys el patrón será _*_, para seleccionar todo:**keys**

# Root Flag

> En este apartado nos solicita que obtengamos la flag que se encuentra "oculta" en el sistema

En el ejercicio anterior nos ha mostrado todas las claves que tiene la base de datos y nos ha aparecido una llamada _flag_ para visualizar el valor de una clave es muy sencillo, simplemente indicamos _get flag_, tras su ejecución vemos que nos facilita la flag: **03e1d2b376c37ab3f5319922053953eb**

___

Con esto concluimos la cuarta máquina del apartado "starting-point" de HTB, nos vemos en el siguiente post con la máquina _Explosion_.

Saludos.

