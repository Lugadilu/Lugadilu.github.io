---
title: "On-Path Attacks  with Ettercap"
excerpt: "Man in the middle attack on a network and rading this using wireshack"
date: 2025-07-22
tags: [MITM, on-path, Network-based vulnerabilities, Wireshark, TCP dump]
layout: single
author_profile: true
---
### Man In The Middle using Ettercap
On-path attacks are very powerful ways to steal data that is travelling on a network. Without end-to-end encryption,
as with much data travelling on local LANs.
it is easy to capture clear text information, and even complete files, using on-path attack methods.

Ettercap is used to perform on-path (MITM) attacks. The goal of an on-path attack is to intercept traffic 
between devices to obtain information that can be used to impersonate the target or to alter data being transmitted

![Topography Image](assets/image/topography.png)

#### step1: setting up ARP spoofing attack
In this attack, you will use ARP spoofing to redirect traffic on the local virtual network to your Kali Linux system at 
10.6.6.1. ARP spoofing is often used to impersonate the default gateway router to capture all traffic entering or leaving
the local IP network. Because your lab environment uses an internal virtual network, instead of spoofing the default gateway, 
you will use ARP spoofing to redirect traffic that is destined for a local server with the address 10.6.6.13.
The target host in this lab is the Linux device at 10.6.6.23. 

use ssh to login and run ssh -l labuser 10.6.6.23 , labuser is the username and insert the password tha you brute-forced.
Use the command **ip neighbor** to view the current ARP cache on the target computer.

#### step2: using wireshark to observe ARP spoofing attack.
Return to the terminal session that is connected via SSH to 10.6.6.23. Ping the IP addresses 10.6.6.11 and 10.6.6.13. 10.6.6.11 is
another host on the LAN that we will verify is unaffected by the attack. Then, use the ip neighbor command to find the MAC 
addresses associated with the IP addresses of the two systems. Record them.
To find the MAC of 10.6.6.23, go to the SSH session terminal and enter the ip address command.

**ettercap -T** command runs Ettercap in text mode, instead of using the GUI interface. 
in a new terminal, run **sudo ettercap -T -q -i br-internal --write mitm-saved.pcap --mitm arp /10.6.6.23// /10.6.6.13//**
where:
- -T means use text only interface
- -q means run in quiet mode
- -i means specify the sniffing/attacking network interface ie br-internal
- --write mean s write package to .pcap file that can be opened in wireshark
- --mitr arp means conduct the ARP poisoning MITM attack
- 10.6.6.23 is the target host, the one generating traffick
- 10.6.6.13 target server, the one we are pretending to be

  #### step3: open wireshark to view the saved PCAP file
  In the Kali terminal window, start Wireshark with the mitm-saved.pcap ie **wireshark mitm-saved.pcap**
  The Ettercap attack computer first broadcasts ARP requests to obtain the actual MAC addresses for the two target hosts,
  10.6.6.23 and 10.6.6.11. The attacking machine then begins to send ARP responses to both target hosts using its own MAC
   for both IP addresses. This causes the two target hosts to address the Ethernet frames to the attacker’s computer,
   which enables it to collect data as an on-path attacker.

  The computer executing the ettercap attack must be located on the same IP network as the target because ARP protocal
  uses Layer 2 broadcasts to obtain the destination MAC associated with the target IP address. Broadcasts and
  Layer 2 ARP information are not forwarded beyond the local network.

  **To prevent this, make sure you advice your client to put policies and configurations to prevent duplicate
  IP addresses on the network.**

  
  
  
  





