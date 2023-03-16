---
layout: post
title: MACHINES 'STARTING POINT' TIER 0 (HTB). REDEEMER (EN)
categories:
- english
- laboratories
tags:
- Hack The Box
- Starting Point
- Machines
- Tier 0
img_path: "/assets/img/posts/20230314/"
image:
  path: portada.png
  alt: REDEEMER
date: 2023-03-07 19:43 +0100
---
Hello everybody,


Today we are going to see the fourth machine of the "very easy" level of the "Starting Point" labs section called [**Redeemer**](https://app.hackthebox.com/starting-point), back to a machine.

# Introduction

Once the machine has been activated, it provides us with the ip _10.129.8.75_, so in situations where this ip is indicated, it must be changed to the one provided by the platform.

> All questions are marked with an asterisk (*) and a letter, which makes it easier to solve the task.
{: .prompt-tip }

# Task

## Task 1

> Which TCP port is open on the machine? 

To find out the open ports we must use the nmap tool with the following commands:

```bash
sudo nmap -p- --open 10.129.8.75
```

- -p-: We indicate that it should scan all the ports, the 65535 ports

- --open: We indicate that we only want to get the open ports.


Once the command is launched, it returns only one open port, **6379**.


## Task 2

> Which service is running on the port that is open on the machine? 

With the above command, we see that you have also provided us with the service running on the port, in this case **redis**.


## Task 3

>  What type of database is Redis? Choose from the following options: (i) In-memory Database, (ii) Traditional Database 

Although the clue is quite clear, if we do a Google search we can see that networks are an **In-memory Database**, this type of DB allows a much faster data analysis compared to the traditional DB, thanks to the fact that its data is stored in the main memory RAM.

## Task 4

>  Which command-line utility is used to interact with the Redis server? Enter the program name you would enter into the terminal without any arguments. 

To work with redis databases, from the command line, there is a tool called **redis-cli**.


## Task 5

>  Which flag is used with the Redis command-line utility to specify the hostname? 

If we search the redis-cli help using the command:

```bash
 redis-cli -help
```

We can see that the parameter used is **-h**.

## Task 6

> Once connected to a Redis server, which command is used to obtain the information and statistics about the Redis server? 

Once connected with the command:

```bash
 redis-cli -h 10.129.8.75
```

with the _help_ command and the tab key we can see all the possible commands and one in particular calls our attention which is called **info** if we hit enter we see that in its summary it provides us with information and statistics about the server.


## Task 7

>  What is the version of the Redis server being used on the target machine? 

If we run the command seen in the previous question, we see that the first line gives us the version: **5.0.7**

## Task 8

> Which command is used to select the desired database in Redis? 

As before with the help parameter, we see that we have a command called **select** whose information indicates that it allows us to select the DB of the connection through its index.

## Task 9
>  How many keys are present inside the database with index 0? 

We choose the database with the command

```bash
 select 0
```
To obtain the number of key-value, we must indicate the command dbsize and it gives us that we have a total of **4** keys.


## Task 10
>  Which command is used to obtain all the keys in a database? 

If we look in the with the help again we see that we have a command called _keys_, but we need a pattern, so if we think in regex patterns we can evaluate that to select all the keys the pattern will be _*_, to select all:**keys**.

# Root Flag

> This section asks us to obtain the flag that is "hidden" in the system.

In the previous exercise it has shown us all the keys that the database has and a call _flag_ has appeared to visualize the value of a key is very simple, we simply indicate _get flag_, after its execution we see that it provides us with the flag: **03e1d2b376c37ab3f5319922053953eb**.
___

This concludes the third machine of the "starting-point" section of HTB, see you in the next post with the _Explosion_ machine.

Cheers.

