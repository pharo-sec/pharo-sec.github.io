---
title: Threat Modeling
date: 2022-09-27 12:00:00 -500 
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

## Service Discovery

This is the next piece of the puzzle, now that we've identified all of our assets we need to identify what's running on them. What ports or services are available on our assets and how are they related (i.e. web applications, databases, etc...)? 

## Vulnerability Assessment

Now that we have all of our assets and services, we start thinking about our attack surface. What are the types of attacks we'd need to look for? what techniques would attacker try to leverage to compromise our network? etc...

## Identifying Threat Actors

We have a (hopefully not) long list of potential attacks vectors and vulnerabilities that we need to address. But we can be smart with our resources by taking a risk based approach to our resources. What I mean by that is, let's say our organization is a financial organization. If I wanted to create a stronger security posture I would want to know the most common attack vectors used against financial organizations, since those would be the most likely tactics/techniques used against our organization.

To access data like that we'll need to use the [MITRE ATT&CK](https://attack.mitre.org/) framework. From there we'll be able to see the groups associated with attacking organizations like ours, we'd want to note they types of tactics and techniques they employ during their attacks so that we have a better understanding of what we can expect if we become the targets of these groups.

We've now used real-world data to determine who the threat actors are and how they would go about attacking our systems. 

## Preventative Security Controls

Now we know the common tactics and techniques used that would be used against us, we need to identify which our previously discovered vulnerabilities overlap with methods used by threat actors and start building controls and mitigations around those attacks. By doing this we can more efficiently spend our time and resources securing against the most likely attack scenarios. 

## Confirming Effective Controls

Finally, we need to make sure that what we've been in place actually works. There are several ways of doing this, the most popular or common would be running a vulnerability scan to confirm simple configuration changes. For a more in depth test you'd want to hire a firm to conduct a penetration test and see how far a potential attacker could get in your environment. 