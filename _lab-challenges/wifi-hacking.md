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
