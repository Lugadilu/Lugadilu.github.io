---
title: "Social Engineering Tool(SET)"
excerpt: "clone a website and obtain user credentials"
date: 2025-07-21
tags: [social engineering, SET]
layout: single
author_profile: true
---
### Social Engineering Tool
To **launch SET toolkit** you need persistent root access so do **sudo -I** and then run **setoolkit**
Select option 1 socio-eng attacks

To clone a website, choose 2 **website attack vectors**, then **Credential Harvester** then **site cloner**
At the prompt, enter the web attacker IP address. This is the external (internet facing) address of the attack computer.

The listener is now active on port 80 on the Kali computer and all port 80 traffic will be redirected to this screen.

#### Capturing and Viewing User Credentials
In a “real-life” exploit, at this point, a phishing exploit containing a link or QR code that sends the user to the fake website is created and sent. In this lab, an html document is created to direct the user to the fake webpage
Enter the URL of the website that you want to clone i. e http://DVWA.vm
Open the Kali Linux Mousepad text editor using the Applications > Favorites > Text Editor choice from the menu. Enter the HTML code shown into the Mousepad document.
<html>
<head>
<meta http-equiv="refresh" content="0; url=http://10.6.6.1/" />
</head>
</html>
Select File > Save from the Mousepad menu. Name the document Great_link.html and save it in the /home/kali/Desktop Folder. The icon appears on the Kali desktop.
Close the Mousepad application.

#### Capture User Credentials.
Double-click the desktop icon for the Great_link.html page to open DVWA login page on a browser
Enter the login credentials, enter and now return to the terminal session running SET

To save the XML, copy the path and open it after you exit ie cat /root/.set/reports/”2023-04-07 17:32:55.967169.xml
