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
![Topography Image](/assets/images/nmap.png)
Since http is open , we can try to reach the site on a web on port 80 ie 10.10.119.61:80. 
**Agent R asks as to Use your own codename as user-agent to access the site.**

To direct yourself to a secret page, the hint usually comes from looking at the HTTP headers. This is like doing a MITM attack.
We use Burpsuite to intercept http request. The server checks the User-Agent string you send.
![Topography Image](/assets/images/burp.png)
A **User-Agent** is a string that your browser (or any HTTP client) sends to a web server in the HTTP request headers.
It tells the server what kind of device/browser is making the request.
Can be used to know the users on the communication

The challenge expects you to change the User-Agent to something else.
Open Burp, navigate to proxy, turn on interception and open browser, When we run the iP again, it will continue loading because
the request goes through burp. 
Right click on the get and forward it to repeater so that we can continue modifying it.
Until you click forward in proxy, and now te server can now return the values.
![Topography Image](/assets/images/c.png)

In repeater, we know their is a user_agent called R, so change it to R and send and observe.
From the hint, we are being asked to tryuse agent C, so we just change R to C.
![Topography Image](/assets/images/c1.png)
Doing this we immediately run into a 302 redirection error and we also discover **agent_C_attention.php** in the location.
In the get / that fetches from home, now paste this agent_C_attention.php and send the request. dont 
modify anything else. And now we get a 200 OK message. Which tell us that C is chris and we have another agent called **J**
It also gives us a hint that **chris password is weak**
![Topography Image](/assets/images/chris.png)

#### Hash-Cacking and Brute-forcing
We know ftp is open on this server, so to get the FTP password, we can use hydra.
Hydra is normally used to guess logins for remote authentication for FTP, SSH, HTTP, Telnet, RDP, SMB , postresql etc
Do hydra -h to see the options. Since we know the user as chris, edit it and use usr/share/wordlists/rockyou as the passlist, and ip and run.
Login into ftp using the name and ip and download the files by doing a get
![Topography Image](/assets/images/ftp.png)

On the local machine, view them and view the txt by using cat.

Run binwalk on each to rty and view hidden information like the txt says. We use binwalk in digital forensics. 
![Topography Image](/assets/images/binwalk.png)
This is **stignography**
Use binwalk to help extact the hidden file by adding an -e flag
if we do this on cutie.png we extract a zip file 8702.zip, but it has a password, so to  open it use john the ripper flag
**zip2john 8702.zip > hash**, this extracts the hash and saes it in a file called **hash**
![Topography Image](/assets/images/cutie_ext.png)
![Topography Image](/assets/images/extracted.png)
The hash can now be used, we crack it again by using john ie **john hash -w=?usr/share/wordlists/rockyou.txt**
the cracked password is **alien**, if already cracked, do **john --show hash** Use it now to extact and read the file
![Topography Image](/assets/images/alien.png)
The text is encoded, and is a base64, gogle cyber chef and use it to decode this . We see  Area51, but we dont know where to use
![Topography Image](/assets/images/cyberchef.png)
Since we are hinted with steg, we can try steg utility ie **steghide extract -sf cute-alien.jpg** we get name and password for james. password **hackerrules!** 
![Topography Image](/assets/images/james.png)
since ssh was open, we can use the above to login to james. and obtain the first flag
![Topography Image](/assets/images/flag1.png)
we also have an image that we can analyze, we use get on ftp but it cant work on SSH, we can only work remotely and try and reach james
On a new window, run **scp james@10.10.221.89:~/Alien_autospy.jpg ./Alien_autospy.jpg** and view it
![Topography Image](/assets/images/downloadedimage.png)

Use reverse image search to upload the image and try to find where and when it was taken.

#### priviledge escalation
we are login into james, and we need to escalate to root.
WE can try to find out the sudo version of this machine, ie **sudo --version** in james account and use chatgpt to give you a command to exploit this and give root access.







