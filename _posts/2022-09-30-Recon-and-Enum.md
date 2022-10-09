---
title: Reconnaissance and Enumeration
date: 2022-09-27 12:00:00 -500 
categories: [PenTesting Methodology]
tags: [pentesting]
---

# Reconnaissance and Enumeration
For a high level description you can look at [this post](https://pharosec.net/posts/Network-Pentesting-Methodology/#reconnaissance) to get an idea of what we'll be talking about. I'll split this post into the two primary sections I'll be discussing: 

1. Manual Enumeration
2. Automated Enumeration

In this post I'll be mostly focusing some of the more common tools and techniques used during the recon phase. and what some of the key things you'll want to identify before moving to the next phase of your engagement

## Goals and Objectives

Through the use of automated and manual techniques, we ultimately want to obtain information that could help give us a more complete picture of our target. This includes information like installed softwares, software/service versions, service banners, operating system information, etc...

Information like this can help us in our later threat modeling phase because we can have a more comprehensive look at what all is available to us and how we may be able to leverage them.

## Manual Enumeration

Manual techniques will vary from service to service, but typically when it comes to open ports you can use netcat to attempt to connect to that port and see how the service responds, and in some cases you can even interact with the service. On web applications, you can search for fields that accept user input, read through server responses, pass special characters to test error handling, etc..

Manual enumeration really boils down to directly interacting with the service/application. Again, a lot of enumeration techniques will vary based on the service, when I come across a service I'm not familiar with I usually go checkout the [HackTrickz](https://book.hacktricks.xyz/generic-methodologies-and-resources/pentesting-methodology) page and see what tools/techniques can be used.

## Automated Enumeration

Automated enumeration varies from manual enumeration in that it relies on tools to directly interact with the service/application rather than the tester. This is a critical component of the enumeration phase because we can run multiple tools at a time, while we tackle other tasks (more manual enumeration).

Starting out with what's probably **the** most popular enumeration tool - Nmap. I can't think of an engagement where I haven't start with a quick Nmap command. This is a great way to quickly find all available ports/services, service versions, and potential exploits all from a single command! Depending on the services found, there are a plethora of tools available. To keep it simple, I'll give some examples of tools that could be used to enumerate web apps, keep in mind, there are an endless number of tools that can be used to enumerate applications. I'll keep it brief and list my personal favorite tools:

1. GoBuster
2. Nikto
3. ffuf
4. Burp Suite
5. sslscan