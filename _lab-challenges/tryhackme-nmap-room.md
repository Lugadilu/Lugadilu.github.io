---
title: "Nmap room on tryhackme"
excerpt: "understanding port scanning, why its necessary, nmap tools"
date: 2025-07-20
tags: [nmap, netacad, reconnaissance, networking]
layout: single
author_profile: true
---

Step by step room to answering the questions given
### 🧠 Key Learnings

-  -sU used for UDP scanning
-  -O switch to detect which operating system the target is running on
- -sV switch to detect the version of the services running on the target
- -p 80 tell nmap to only scan port 80
- -p- tell nmap to scan all ports
- --script=vuln activate all of the scripts in the "vuln" category
- -sn switch tells Nmap not to scan any ports

  #### Three-way handshake
It consists of three stages. 
- The connecting terminal (our attacking machine) sends a TCP request to the
  target server with the SYN flag set.
- The server then acknowledges this packet with a TCP response containing the SYN flag,
   as well as the ACK flag.
- our terminal completes the handshake by sending a TCP request with the ACK flag set.

  TCP Connect scan works by performing the three-way handshake with each target port in turn.
   Nmap tries to connect to each specified TCP port,
  and determines whether the service is open by the response it receives.
   If the connection does not exist (CLOSED), then a reset is sent in response to any incoming segment except another reset

When a UDP port is closed, by convention the target should send back a "port unreachable" message using ICMP

**Nmap Scripting Engine (NSE)** can be used for scanning for vulnerabilities, to automating exploits for them.
To search for scripts, enter into the scripts directtory ie /usr/share/nmap/scripts/scripts then grep /usr/share/nmap/scripts/script.db

#### vulnerability scanning
The command is nmap -sV --script vulners --script-args mincvss=4 10.6.6.23
where script argument mincvss restricts the output to only those CVEs that 
have a higher CVSS score than the one specified in the argument.

