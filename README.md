# Monitoring 101

[See report monitoring](./report_monitoring.md)

## Goals

+ How to monitor a Linux system 
+ What to look for ?

**Questions to answer**
+ What are the main area of concern when monitoring a system? (EX: CPU load, disk usage, ...)
+ How can you check what are the most memory intensive running processes ?
+ What are log files? Where can you find them on a typical Linux system ?
+ How can you check who where the last connected users, what they did, when they left ?
+ What are the different metrics of health and performance of a system ?
+ How can you check the uptime of a machine ?
+ How can you assess the network traffic ?

## Processor and processes

`ps` to display 


## Linux system monitoring tools
+ `top ` command displays linux processes in CPU activity order, installed by default
+ `htop`, a better top, needs to be installed, uses ==**ncurses**== for its displays 


## Log files

We can find the logs of a linux system in the directory /var/log.


### rsyslog

**rsyslog** is the service responsible for handling logs in Ubuntu.
rsyslog configuration is in the file /etc/rsyslog.conf

`grep "/var/log" /etc/rsyslog.conf`

Centralize logs of several machines on one server

On my ubuntu vm-client, I just have the folder /etc/rsyslog.d 
On my ubuntu server, I have the file /etc/rsyslog.conf and the folder /etc/rsyslog.d 

### Resources
FR: [Analysez les principaux fichiers de traces](https://openclassrooms.com/fr/courses/7274161-administrez-un-systeme-linux/7529366-analysez-les-principaux-fichiers-de-traces)
FR: [Centralisation des logs sous linux](https://www.linuxtricks.fr/wiki/rsyslog-centralisation-des-logs-sous-linux)

## Users connections and activity
*How can you check who where the last connected users, what they did, when they left ?*

### /var/log/auth.log


### `last`

<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

- [Monitoring 101](#monitoring-101)
  - [Goals](#goals)
  - [Processor and processes](#processor-and-processes)
  - [Linux system monitoring tools](#linux-system-monitoring-tools)
  - [Log files](#log-files)
    - [rsyslog](#rsyslog)
    - [Resources](#resources)
  - [Users connections and activity](#users-connections-and-activity)
    - [/var/log/auth.log](#varlogauthlog)
    - [`last`](#last)
    - [`lastlog`](#lastlog)
    - [`who` and `w`](#who-and-w)
    - [Resources](#resources-1)
  - [Network activity](#network-activity)
    - [`ss`](#ss)
  - [For next project](#for-next-project)

<!-- /code_chunk_output -->


The last command pulls its data from /var/log/wtmp.
The file /var/log/wtmp is updated each time a user logs in.   
It'll show username, tty, IP address, date and time, and session start/stop times.

### `lastlog`

With the command `lastlog`, we can see _when_ each user last logged in.
Those information comes from the file `/etc/log/lastlog` They are sorted based on the user entries from the file /etc/password entries.

**To see last login of root user**
`lastlog -u root`

An attacker with access to root can modify or delete the authentication logs.
The absence of authentication logs can be suspect.

### `who` and `w`
With the command `who` we can see which user is connected, we can also see the date of the last reboot.

La commande w effectue en fait un condensé de plusieurs autres commandes permettant de relever notamment l'activité du processeur. 

### Resources
[How to view authentication logs on Ubuntu](https://bitlaunch.io/blog/how-to-view-authentication-logs-on-ubuntu-20-04/)

## Network activity


### `ss` 
`ss -lptun` = modern replacement to `netstat`
`ss` stands for socket statistics

==**explain what a socket is**== 

## For next project

https://www.it-connect.fr/linux-recevoir-un-e-mail-lors-dune-connexion-ssh/