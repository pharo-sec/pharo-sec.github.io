---
title: HTTPyLogger - A Python Keylogger
date: 2022-09-15 20:00:00 -500 
categories: [Projects]
tags: [python, malware]
---
**Disclaimer: This is purely for educational purposes only.**

# HTTPyLogger

This is my implementation of a keylogger in Python that exfiltrates the logged keystrokes via GET requests to the attacker's C2 server. The code can be found [here](https://github.com/pharo-sec/HTTPyLogger)

## Setup

Run:
```shell
$ pip3 install -r requirements.txt
```

## Usage

Running the script requires root access:
```shell
$ sudo ./HTTPyLogger.py
```

Change the URL in the script to the address of the C2 server. in my case I'll be demonstrating it with 127.0.0.1:8080.

Setup the HTTP server:
```shell
$ sudo python3 -m http.server 8080
```
To generate traffic, start typing as usual. When the "Enter" key is pressed, a base64 encoded string of the logged keys will be sent to the attacker's server

![HTTP Logs](/assets/HTTPyLogger/http-logs.png)

We can now take these strings, decode them, and access the victim's keystrokes:

![HTTP Logs](/assets/HTTPyLogger/decoded-strings.png)