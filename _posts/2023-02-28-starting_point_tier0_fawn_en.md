---
layout: post
title: MACHINES 'STARTING POINT' TIER 0 (HTB). FAWN (EN)
categories:
- english
- laboratories
tags:
- Hack The Box
- Starting Point
- Machines
- Tier 0
img_path: "/assets/img/posts/20230228/"
image:
  path: portada.png
  alt: FAWN
date: 2023-02-28 18:40 +0100
---
Hola a todos,

This time we will be solving the tasks of the second machine of the "Starting Point" laboratories called [**Fawn**](https://app.hackthebox.com/starting-point)

# Introduction

Once the machine has been activated, it provides us with the ip _10.129.155.17_, so in situations where this ip is indicated, it must be changed to the one provided by the platform.

> All questions are marked with an asterisk (*) and a letter, which makes it easier to solve the task.
{: .prompt-tip }

# Task

## Task 1

> What does the 3-letter acronym FTP stand for?

FTP stands for **File Transfer Protocol**, a protocol that, as its name suggests, allows the transfer of files between devices.


## Task 2

> Which port does the FTP service listen on usually? 

Generally, unless the service configuration has been modified, the FTP protocol listens on port **21**.


## Task 3

> What acronym is used for the secure version of FTP? 

By default, the FTP protocol does not offer "security" and data travels in plain text, but its secure version, Secure File Transfer Protocol or **SFTP**, makes an implementation where it encrypts the data being exchanged.


## Task 4

> What is the command we can use to send an ICMP echo request to test our connection to the target? 

TWe saw this in the previous post, remember that the best known tool to launch ICMP frames is **ping**.

## Task 5

> From your scans, what version is FTP running on the target? 

In this task we start interacting with the machine, and we will see the version it is running by launching the nmap tool:

```bash
 nmap -sV -p21 10.129.155.17
```
- -sV: Allows us to find the version of the service.
- -p21: It is only used to focus on the indicated port.

As you can see the service running is **vsftpd 3.0.3**.


![nmap](nmap-1.png){: width="972" height="589" }
_nmap ftp_

## Task 6

> From your scans, what OS type is running on the target?

To find the operating system of a computer we can use nmap together with its -O variable to get the operating system but in this case the command needs administrator permissions, this is achieved by launching the tool with sudo.

> There are other ways to obtain the operating system with this tool. [**Man Nmap**](https://nmap.org/book/man-os-detection.html)
{: .prompt-tip }

```bash
 sudo nmap -O 10.129.155.17
```
In this case you can see that with the command launched it is not possible to obtain the expected result, so we must launch the command with a little help, trying to make nmap try to guess the operating system.

```bash
 sudo nmap -O --osscan-guess 10.129.155.17
```

In this case we see that you get several options:

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

In this case we see that the almost 100% is a Linux operating system, however, if we try to enter the result we see that it gives us an error, however, they give us a clue, the answer only has 4 letters and as we well know all or almost all Linux operating systems are based on **unix**, which will be the correct answer.

## Task 7

> What is the command we need to run in order to display the 'ftp' client help menu?

In order to use the `ftp` tool we must install it on our system, using the command:

```bash
 sudo apt install ftp
```

> The method of installing the tool will depend on your operating system.
{: .prompt-warning }

Like most tools it follows an unwritten convention in which the tool's help commands can be launched using the following command variables:

- -m
- -h
- -s
- help
- man

In this case the command to use is **ftp -h**.

![ftp](ftp-1.png){: width="972" height="589" }
_ftp_


## Task 8

> What is username that is used over FTP when you want to log in without having an account?

If we look for information about the [**ftp**](https://www.uv.es/bombinp/ftp.html) service, we can see that there is a user that is automatically created in the service when it is installed on a machine, by means of the configuration it is possible to cancel the entry of this user to the FTP service, this user is **anonymous**.


## Task 9

> What is the response code we get for the FTP message 'Login successful'? 

If we try to connect as the user anonymous to the service offered by the machine using the command:

```bash
  ftp 10.129.155.17
```

In the user we will have to enter _anonymous_ and the password can be left empty.


As you can see before the string Login successful, we see the code **230**.

![ftp](ftp-2.png){: width="972" height="589" }
_ftp_


## Task 10

> There are a couple of commands we can use to list the files and directories available on the FTP server. One is dir. What is the other that is a common way to list files on a Linux system. 

It is well known that the command for listing files in Linux is **ls**. More information can be found in this [**link**](https://linuxize.com/post/how-to-list-files-in-linux-using-the-ls-command/)

## Task 11

> What is the command used to download the file we found on the FTP server? 

If we look in the ftp command help once connected we can see the different commands:


![ftp](ftp-3.png){: width="972" height="589" }
_ftp_



If we look for information about what each of the commands does ([**link**](https://www.serv-u.com/linux-ftp-server/commands)), we can see that to get a file we must use the **get** command.


# Root Flag

> This section asks us to obtain the flag that is "hidden" in the system.

To get the flag we must search for it on the system to which we have access.

Once connected we can list the content of the folder to which we have access as anonymous users, and see that there is a file called _flag.text_. We can get it by calling the _get_ command and once downloaded display it with the _cat_ command:

```bash
 get flag.txt
 exit
 cat flag.txt
```
After executing these commands it can be verified that the flag is: **035db21c881520061c53e0536e44f815**

![flag](flag.png){: width="972" height="589" }
_flag_
___

This concludes the second machine of the "starting-point" section of HTB, see you in the next post with the _Dancing_ machine.

Cheers.

