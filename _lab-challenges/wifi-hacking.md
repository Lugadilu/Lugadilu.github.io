---
title: "Wifi Hacking"
excerpt: "attacking WPA1/WPA2 Network"
date: 2025-07-26
tags: [Wireless Networks, Wifi]
layout: single
author_profile: true
---
### Introduction
The core of WPA(2) authentication is the 4 way handshake.

Most home WiFi networks, and many others, use WPA(2) personal. If you have to log in with a password and it's not WEP, then it's WPA(2) personal. WPA2-EAP uses RADIUS servers to authenticate, so if you have to enter a username and password in order to connect then it's probably that.

Previously, the WEP (Wired Equivalent Privacy) standard was used. This was shown to be insecure and can be broken by capturing enough packets to guess the key via statistical methods.

The 4 way handshake allows the client and the AP to both prove that they know the key, without telling each other. WPA and WPA2 use practically the same authentication method, so the attacks on both are the same.

After snatching the passwords and BSSId, we can bruteforce to know the passwords

Using the Aircrack-ng suite, we can start attacking a wifi network. This will walk you through attacking a network yourself, assuming you have a monitor mode enabled NIC.

The aircrack-ng suite consists of:

aircrack-ng
airdecap-ng
airmon-ng
aireplay-ng

You also need to ensure you have a monitor mode Network Interface Card (NIC) to capture the 4-way handshake used by WPA networks. If you need results fast, it is good to use injection mode, which de-authenticates a client from the WIFI and forces the handshake to re-occur as the client tries to reconnect to the network

#### Analysis of the traffic to crack the password

We assume you have collected the traffic and stored it into a file to be conveerted by aircrack-ng.
In this tutorial , we will download the already collected file on tryhackme ie https://tryhackme.com/room/wifihacking101.

On your kali linux , its in the downloads with a name extention like Captures_1578171018678.tar.gz. Its unzipped so it needs to be unzipped y running the command tar then name of the file ie tar xyz Captures_1578171018678.tar.gz

After this, use aircrack-ng to prepare the unzipped NinjaJc01-01.cap file that will help us crack the password by running aircrack-ng -j wifi NinjaJc01-01.cap.
this simply mean:
 - -j create a HCCAPX in order to use hashcat to crack the password
 -  wifi is the new file name
 -  Ninja is the capfile we unzipped

From the above information, the name of the wifi is listed at the ESSID ie James Honor 8

The last thing to do is cracking this wifi2.hccapx file we just generated, we do a command like aircrack-ng -a2 -b 02:1A:11:FF:D9:BD -w /usr/share/wordlists/rockyou.txt wifi2.hccapx and run where 
 - a2 sets the attack mode as dictionary attack. sends the tool
 - -b illustrates the mac address ie 02:1A:11:FF:D9:BD of the router
 - -w specifies the wordlist

it runs for a while and vwala we have the password as greeneggsandham 
