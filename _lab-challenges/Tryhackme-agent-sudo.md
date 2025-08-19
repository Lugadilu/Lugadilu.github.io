---
title: "Agent sudo CTF"
excerpt: "challenge that teaches privilege escalation using Sudo misconfigurations"
date: 2025-08-19
tags: [Nmap, Burp Sute, Privilege Escalation]
layout: single
author_profile: true
---
### Agent Sudo Introduction
You are tasked with investigating a secret agent server. The "agent" has left some clues, and your goal is to gain initial access, 
escalate privileges, and eventually read the root flag.
Agent Sudo is about finding a way in (via enumeration + brute force) and then exploiting sudo misconfigurations to get root.
Their is also steganography, to extract hidden data from an image

#### Enumeration
Connect the machine to your local machine via the vpn, ie cd Downloads, sudo openvpn Lugadilu.ovpn
Run an Nmap Scan to identify open ports Ie nmap -sV -sC IP to discover ftp, ssh and http are open and running on the machine.

Since http is open , we can try to reach the site on a web on port 80 ie 10.10.119.61:80. 
**Agent R asks as to Use your own codename as user-agent to access the site.**

To direct yourself to a secret page, the hint usually comes from looking at the HTTP headers. This is like doing a MITM attack.
We use Burpsuite to intercept http request. The server checks the User-Agent string you send.
A **User-Agent** is a string that your browser (or any HTTP client) sends to a web server in the HTTP request headers.
It tells the server what kind of device/browser is making the request.
Can be used to know the users on the communication

The challenge expects you to change the User-Agent to something else.
Open Burp, navigate to proxy, turn on interception and open browser, When we run the iP again, it will continue loading because
the request goes through burp. 
Right click on the get and forward it to repeater so that we can continue modifying it.
Until you click forward in proxy, and now te server can now return the values.

In repeater, we know their is a user_agent called R, so change it to R and send and observe.
From the hint, we are being asked to tryuse agent C, so we just change R to C.
Doing this we immediately run into a 302 redirection error and we also discover **agent_C_attention.php** in the location.
In the get / that fetches from home, now paste this agent_C_attention.php and send the request. dont 
modify anything else. And now we get a 200 OK message. Which tell us that C is chris and we have another agent called **J**
It also gives us a hint that **chris password is weak**







