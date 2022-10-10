---
title: "Oh my web server"
description: Writeup of Oh my web server from TryHackme, difficulty medium.
slug: "ohmywebserver"
date: 2022-10-10T09:25:01-04:00
draft: false
categories:
    - TryHackMe
tags:
    - Linux
    - Web-Exploitation
    - Medium
---

Started with a simple nmap scan.

It shows  ports open, 22(ssh) and 80(http).

![nmap output](/img/ohmywebserver/nmapscan.png)

I first went to the website and it showed a generic website for a consultancy business.

![website home page](/img/ohmywebserver/homepage.png)

I did some manual enumeration and couldn't find anything so I ran gobuster but also didn't find anything of use.

![gobuster output](/img/ohmywebserver/gobuster.png)

I went back and looked at the nmap scan and apache 2.4.49 was in use so I searched for any exploits. I found [this](https://www.exploit-db.com/exploits/50383) directory traversal and remote code execution exploit.

![exploit](/img/ohmywebserver/exploit.png)

I ran this command, using the exploit to get a reverse shell.

![exploit used](/img/ohmywebserver/exploitused.png)

Using netcat I set up a listener.

![netcat](/img/ohmywebserver/netcat.png)

I noticed it was inside of a docker container as shown by the ".dockerenv". I searched for a flag but I couldn't find one.

![file list](/img/ohmywebserver/filelist.png)

I tested if it was mounted or not, it was not.

![docker mounted check](/img/ohmywebserver/dockermounted.png)

I found that it had python3 capabilities.

![capability check](/img/ohmywebserver/capabilitycheck.png)

First thing I did was go to [gtfobins](https://gtfobins.github.io/gtfobins/python/) and read the capabilities section.

![gtfobins](/img/ohmywebserver/gtfobins.png)

I used that and then stabilised the shell.

![docker root](/img/ohmywebserver/dockerroot.png)

I got the user flag from /root/user.txt

![userflag](/img/ohmywebserver/userflag.png)

Question: What is the user flag? Answer: THM{eacffefe1d2aafcc15e70dc2f07f7ac1}

I further enumerated the docker container so that I can try and escape it.

![if config](/img/ohmywebserver/ifconfig.png)

I grabbed nmap from [here](https://github.com/andrew-d/static-binaries/raw/master/binaries/linux/x86_64/nmap)

I set up a http server using python so that I could get nmap onto the docker container.

![http server](/img/ohmywebserver/pythonserver.png)

I got nmap onto the docker container and did a scan. The scan showed two interesting ports, 5985 and 5986.

![docker nmap scan](/img/ohmywebserver/dockernmapscan.png)

I looked up the ports on [hacktricks](https://book.hacktricks.xyz/network-services-pentesting/5985-5986-pentesting-omi)

![hack tricks](/img/ohmywebserver/hacktricks.png)

I found [this](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2021-38647) vulnerability.

Then when looking for an exploit I found [this](https://github.com/AlteredSecurity/CVE-2021-38647)

I then used the same method I did for nmap for the exploit.

![exploit upload](/img/ohmywebserver/placingexploit.png)

I tested the exploit with the command "whoami" and it was sucessful.

![exploit test](/img/ohmywebserver/exploittest.png)

I then used the exploit read the root flag.

![root flag](/img/ohmywebserver/rootflag.png)

Question: What is the root flag? Answer: THM{7f147ef1f36da9ae29529890a1b6011f}


