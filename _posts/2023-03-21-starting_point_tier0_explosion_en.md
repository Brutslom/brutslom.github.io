---
layout: post
title: MACHINES 'STARTING POINT' TIER 0 (HTB). EXPLOSION (EN)
categories:
- english
- laboratories
tags:
- Hack The Box
- Starting Point
- Machines
- Tier 0
img_path: "/assets/img/posts/20230321/"
image:
  path: portada.png
  alt: EXPLOSION
date: 2023-03-21 00:00 +0000
---
Hello everybody,


Today we are going to see the fourth machine of the "very easy" level of the "Starting Point" labs section called [**Explosion**](https://app.hackthebox.com/starting-point), where we will be looking at a Windows machine.

# Introduction

Once the machine has been activated, it provides us with the ip _10.129.243.76_, so in situations where this ip is indicated, it must be changed to the one provided by the platform.

> All questions are marked with an asterisk (*) and a letter, which makes it easier to solve the task.
{: .prompt-tip }

# Task

## Task 1

>  What does the 3-letter acronym RDP stand for? 

If we search the internet for the meaning of this acronym we find its value **Remote Desktop Protocol**, it is a protocol for accessing a remote desktop.


## Task 2

>  What is a 3-letter acronym that refers to interaction with the host through a command line interface? 

The acronym by which it is usually referred to when interacting with the console is **CLI**.


## Task 3

>  What about graphical user interface interactions?  

The acronym used when using the graphical user interface is **GUI**.

## Task 4

>  What is the name of an old remote access tool that came without encryption by default and listens on TCP port 23?

Generally the service that runs under port 23 is **telnet**, which is a protocol that allows the management of a remote machine.


## Task 5

>  What is the name of the service running on port 3389 TCP? 

If we run the command:

```bash
nmap -sV -p3389 10.129.243.76

```

- -sV: Allows to find the version of the service.
- -p3389: It is only used to focus on the indicated port.


Once the command is launched we see that the service running under that port is **ms-wbt-server**.

## Task 6

> What is the switch used to specify the target host's IP address when using xfreerdp? 

xfreerdp is an obsolete tool but if we look in its help we can see that to select the destination host we can use the parameter: **/v:**.


## Task 7

>  What username successfully returns a desktop projection to us with a blank password? 

To find out which user can log in without a password, we can identify the most popular default usernames by using the [**seclist**](https://github.com/danielmiessler/SecLists/blob/master/Usernames/top-usernames-shortlist.txt) tool, where we find the following users:

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

If we test the connection with the different users we can see that with the user **administrator** we can access.


# Root Flag

> This section asks us to obtain the flag that is "hidden" in the system.

Once we have entered the target machine we see a file called flag containing the following text **951fa96d7830c451b536be5a6be008a0**.

___

This concludes the third machine of the "starting-point" section of HTB, see you in the next post with the _Preignition_ machine.

Cheers.

