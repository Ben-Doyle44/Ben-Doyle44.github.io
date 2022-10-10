---
title: "SimpleCTF"
description: Writeup of the SimpleCTF from TryHackme, difficulty easy.
slug: "SimpleCTF"
date: 2022-10-08T13:15:46-04:00
draft: false
categories:
    - TryHackMe
tags:
    - Linux
    - Web-Exploitation
    - Easy
---

I started the the box with a simple nmap scan to know what ports were open and what was running on them.

It shows 3 ports open, 21(ftp), 80(http) and 2222(ssh).

![nmap output](/img/simplectf/nmapscan.png)

Question: How many services are running under port 1000? Answer: 2

Question: What is running on the higher port? Answer: ssh

As shown by the nmap scan the ftp port has anonymous login enabled. I looked at that first. 

![ftp anonymous login and file grab](/img/simplectf/ftp.png)

Upon logging into the ftp service, I found a directory called "pub" and within that a text file called "ForMitch.txt". I used the "get" command to copy the file onto my local machine.

![reading file taken from ftp](/img/simplectf/formitch.png)

The "ForMitch.txt" file alludes to a weak password for the system user.

Nothing more to be found with the ftp login, so I moved onto the website running on port 80.
What's found at first is the default apache2 page.

So I started enumarting the website using gobuster to find directories.

![gobuster output](/img/simplectf/gobuster.png)

/simple seems interesting. It shows a cms homepage and at the bottom it displays which version of the cms software it is using.

![simple version](/img/simplectf/simpleversion.png)

I searched for known vulnerabilities on that version and found this. It contains a working exploit written in python.

[CVE-2019–9053](https://www.exploit-db.com/exploits/46635)

![simple vuln](/img/simplectf/simplevuln.png)

Question: What’s the CVE you’re using against the application? Answer: CVE-2019–9053

Question: To what kind of vulnerability is the application vulnerable? Answer: Sqli

I ran the python script with these paramaters:

![running python script](/img/simplectf/pythonscript.png)

It managed to find the username, email and password.

![exploit output](/img/simplectf/exploitoutput.png)

Question: What's the password? Answer: secret

I'll try to login in via ssh with these credentials.

![ssh login](/img/simplectf/sshlogin.png)

It's sucessful so I'll now try to find the user flag.

![user flag](/img/simplectf/userflag.png)

Question: What's the user flag? Answer: G00d j0b, keep up!

Question: Is there any other user in the home directory? What’s its name? Answer: sunbath

I used sudo -l to see what mitch is able to run.

![sudo -l](/img/simplectf/sudo-l.png)

As vim can run as sudo, I looked at vim on [gtfobins](https://gtfobins.github.io/gtfobins/vim/)

![vim sudo](/img/simplectf/vimsudo.png)

Question: What can you leverage to spawn a privileged shell? Answer: vim

I used sudo vim -c ':!/bin/sh' to get root and then read the root flag in root/root.txt

![root flag](/img/simplectf/rootflag.png)

Question: What's the root flag? Answer: W3ll d0n3. You made it!
