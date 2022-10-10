---
title: "Anthem"
description: Writeup of the boiler CTF from TryHackme, difficulty easy.
slug: "anthem"
date: 2022-10-10T05:48:00-04:00
draft: false
categories:
    - TryHackMe
tags:
    - Windows
    - Web-Exploitation
    - Easy
---

As with every box I started with an nmap scan.

It shows 2 ports open, 80(http) and 3389(rdp)

![nmap output](/img/anthem/nmapscan.png)

Question: What port is for the web server? Answer: 80

Question: What port is for remote desktop service? Answer: 3389

I first went to the website as I didn't have credentials for rdp.

![anthem home page](/img/anthem/anthemhomepage.png)

I started with manual enumeration by going to /robots.txt which provided a possible password and 4 urls.

![robots.txt](/img/anthem/robots.png)

Question: What is a possible password in one of the pages web crawlers check for? Answer: UmbracoIsTheBest!

Question: What CMS is the website using? Answer: Umbraco

Question: What is the domain of the website? Answer: anthem.com

I went back to the home page to see what other information I could gather. On the post "we are hiring" I found a potential email.

![we are hiring post](/img/anthem/wearehiring.png)

I moved onto the second post "a cheers to our it department". It alludes to an admin but doesn't directly name them.

![a cheers to it department](/img/anthem/acheerstoit.png)

I searched the poem up and got the name "Solomon Grundy".

![poem search](/img/anthem/poem.png)

Question: What's the name of the Administrator? Answer: Solomon Grundy

With this name we can assume his email address has the same format as the other one, the first letter of their first and last name followed by @anthem.com.

Question: Can we find find the email address of the administrator? Answer: SG@anthem.com

I checked the source code of the site for the first 4 flags using curl to send and recieve data and grep to find the flag within it.

![flag1 and flag2](/img/anthem/flag1+2.png)

Question: What is flag 1? Answer: THM{L0L_WH0_US3S_M3T4}

Question: What is flag 2? Answer: THM{G!T_G00D}

![flag3](/img/anthem/flag3.png)

Question: What is flag 3? Answer: THM{L0L_WH0_D15}

![flag4](/img/anthem/flag4.png)

Question: What is flag 3? Answer: THM{AN0TH3R_M3TA}

With that finished, I moved onto logging into umbraco using the "sg@anthem.com" and "UmbracoIsTheBest!".

![umbraco home page](/img/anthem/umbracohomepage.png)

I didnt find anything of use so I went onto trying to log in via rdp. I used [remmina](https://remmina.org/) for this 

I logged in using the username "sg" and the password "UmbracoIsTheBest!"

![desktop](/img/anthem/desktop.png)

I then opened the user.txt file giving me the user flag.

![user flag](/img/anthem/userflag.png)

Question: Gain initial access to the machine, what is the contents of user.txt? Answer: THM{N00T_NO0T}

I looked around for any interesting files and came accross the directory backup.

![files](/img/anthem/files.png)

Inside backup was a text file called "restore" but I didn't have permission to open it.

![restore permissions](/img/anthem/restoreperm.png)

I did however, have permission to change the permissions on it.

![restore properties](/img/anthem/properties.png)

I clicked on the edit button, then add and then add sg in the "enter the object name to select" area.

![user-group](/img/anthem/user-group.png)

Then clicked allow full control over the restore file to the user SG.

![adding perms](/img/anthem/addingperms.png)

Opening it gives us "ChangeMeBaby1MoreTime".

![root password](/img/anthem/rootpass.png)

Question: Can we spot the admin password? Answer: ChangeMeBaby1MoreTime

I then looked for the administrator's files to get the root flag

![admin area](/img/anthem/adminarea.png)

I found the root file on the admin's dekstop.

![admin dekstop](/img/anthem/admindekstop.png)

Opening that file gives us the final flag.

![root flag](/img/anthem/rootflag.png)

Question: Escalate your privileges to root, what is the contents of root.txt? Answer: THM{Y0U_4R3_1337}


