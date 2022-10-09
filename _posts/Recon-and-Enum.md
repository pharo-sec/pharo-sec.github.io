---
title: Reconnaissance and Enumeration
date: 2022-09-27 12:00:00 -500 
categories: [PenTesting Methodology, Network Methodology]
tags: [oscp,linux,windows,pentesting]
---

# Reconnaissance and Enumeration
For a high level description you can look at [this post](https://pharosec.net/posts/Network-Pentesting-Methodology/#reconnaissance) to get an idea of what we'll be talking about. I'll split this post into the two primary sections I'll be discussing: 

1. Manual Enumeration
2. Automated Enumeration

In this post I'll be mostly focusing some of the more common tools and techniques used during the recon phase. and what some of the key things you'll want to identify before moving to the next phase of your engagement

## Manual Enumeration

## Automated Enumeration

Starting out with what's probably **the** most popular enumeration tool - Nmap. I can't think of an engagement where I haven't start with a quick Nmap command. This is a great way to quickly find all available ports/services, service versions, and potential exploits all from a single command! 

The most common service you'll likely come across is HTTP/S, there are an endless number of tools that can be used to enumerate applications. I'll keep it brief and list my personal favorite tools and a description of each one:

1. GoBuster: A fast and easy way to bruteforce directories on an application. There are a few alternatives that I've used in the past including dirb, dirbuster, and dirsearch
2. Nikto: An application vulnerability scanner, this is a heavy tool and can take some time to complete depending on the size of the target.
3. ffuf: 
4. 