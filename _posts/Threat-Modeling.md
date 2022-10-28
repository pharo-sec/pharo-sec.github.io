---
title: Threat Modeling
date: 2022-10-24 12:00:00 -500 
categories: [PenTesting Methodology]
tags: [pentesting]
---

# Threat Modeling

## What is Threat Modeling 

From a high level, threat modeling can be summarized as the following:

- Asset Discovery (What do I have?)
- Service Discovery (What does it do?)
- Vulnerability Assessment (What could go wrong?)
- Identifying Threat Actors (Who would want to attack me?)
- Preventative Security Controls (How can I prevent that?)
- Confirming Effective Controls (How can I be sure my prevention works?)

Let's break each of these down and get a better idea of why they're important and how they're connected.

## Asset Discovery

This stage is pretty straightforward, we can't secure something we don't know about. If we don't know about it, it gets left behind and forgotten, making it easy prey for attackers looking for a way in. 

Properly identifying all of our assets can be pretty burdensome at first depending on how well documented our environment is. Generally we want to start at the highest level (physical sites, cloud providers, etc...) and work our way down to identify each subnet. Once that list has been compiled, there are several options available for discovery scanning. You can use a free tool like Nmap and run a discovery scan of all of the identified networks, or opt for a paid platform like Qualys that allows you to schedule network maps on a daily/weekly/monthly basis.

## Service Discovery

Service discovery goes hand in hand with asset discovery since you'll typically be running some kind of port scan during your asset discovery. Once we've our assets we need to identify what ports/services are running on them (i.e. web application, SQL server, FTP server, etc...).

To do that we an use the same tools used during our initial asset discovery but configure the tool to run a full port scan rather than a lighter and faster default port scan (default ports usually includes the top 100 common ports). 

Again, for this task I prefer using Nmap since it's easier to configure and launch, but tools like Nessus and Qualys also work great since they'll feed directly into the next portion of our modeling process.

## Vulnerability Assessment

Now that we have all of our assets and services we can begin looking into potential vulnerabilities or ares of weakness. During this phase we'll want to try to identify the biggest vulnerabilities that affect our organization. 

A really easy to way to quickly enumerate your existing vulnerabilities would be to use a vulnerability scanner like OpenVAS (a free vulnerability scanner), Nessus (which has a community edition for free), or Qualys. All of these options work great and will identify most vulnerabilities. 

The only thing scanners and other automated tools won't find are vulnerabilities that require manual inspection or testing like exposed admin credentials on a file share or a sneaky SQL injection on an account registration page. To identify vulnerabilities like these you'd need to hire a security firm to conduct a penetration test against your networks/applications. 

Once you've compiled your various reports detailing the identified vulnerabilities, you can take that information and do a risk analysis to identify which vulnerabilities pose the largest risk to your organization.

## Identifying Threat Actors

At this point, we should have a list of potential attack vectors and vulnerabilities that we need to address. But we can be smart with our resources by taking a risk based approach to our resources. What I mean by that is, let's say our organization is a financial organization. If I wanted to create a stronger security posture I would want to know the most common attack vectors used against financial organizations, since those would be the most likely tactics/techniques used against our organization.

To access data like that we'll need to use the [MITRE ATT&CK](https://attack.mitre.org/) framework. From there we'll be able to see the groups associated with attacking organizations like ours, we'd want to note they types of tactics and techniques they employ during their attacks so that we have a better understanding of what we can expect if we become the targets of these groups.

We've now used real-world data to determine who the threat actors are and how they would go about attacking our systems. 

## Implementing Preventative Security Controls

Now we know the common tactics and techniques used that would be used against us, we need to identify which our previously discovered vulnerabilities overlap with methods used by threat actors and start building controls and mitigations around those attacks. By doing this we can more efficiently spend our time and resources securing against the most likely attack scenarios. 

## Confirming the Efficacy of the Controls

Finally, we need to make sure that what we've been in place actually works. There are several ways of doing this, the most popular or common would be running a vulnerability scan to confirm simple configuration changes. For a more in depth test you'd want to hire a firm to conduct a penetration test and see how far a potential attacker could get in your environment. 