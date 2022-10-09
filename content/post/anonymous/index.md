---
title: "Anonymous"
description: Writeup of anonymous from TryHackme, difficulty medium.
date: 2022-10-09T09:05:03-04:00
draft: false
categories:
    - TryHackMe
tags:
    - Linux
    - Medium
---
Starting with an nmap scan to see what ports are open and what is running on them.

![nmap output](/img/anonymous/nmapscan.png)

It shows 4 ports open, 21(ftp), 22(ssh), 139(smb) and 445(smb).

Question: Enumerate the machine.  How many ports are open? Answer: 4

Question: What service is running on port 21? Answer: ftp

Question: What service is running on ports 139 and 445? Answer: smb

Since anonymous login is enabled on the ftp port, I tried that first.

![ftp file grab](/img/anonymous/ftpfilegrab.png)

Looking through the scripts directory it seems there is a bash script and 2 text files so I grabbed them all.
The scripts directory is also writeable.

![clean bash script](/img/anonymous/cleanscript.png)

This is a bash script that delete files from the /tmp/ 

The to_do.txt file is just a note saying to remove anonymous login.

The removed_files.log file is a log for clean.sh

With nothing left to look at on the ftp port, I moved onto the smb service with another anonymous login.

![smb login](/img/anonymous/smblogin.png)

Question: There's a share on the user's computer.  What's it called? Answer: pics

I accessed the pics share.

![smb pics](/img/anonymous/smbpics.png)

I grabbed by images and used stegseek to crack any possible steganography passwords.

![stegseek attempt](/img/anonymous/stegseekattempt.png)

Despite trying 10million passwords none worked so I assumed there was nothing hidden in the images.

I knew the scripts directory was writeable so I edited change.sh to include a reverse shell.

![reverse shell script](/img/anonymous/reverseshellscript.png)

I then uploaded that to the scripts directory

![reverse shell upload](/img/anonymous/reverseshellupload.png)

I set up netcat to listen for the reverse shell and gained user access.

![netcat](/img/anonymous/netcat.png)

Then I stabilised the shell.

![stabilise shell](/img/anonymous/stabaliseshell.png)

I got the user flag

![user flag](/img/anonymous/userflag.png)

Question: user.txt Answer: 90d6f992585815ff991e68748c414740

I looked for suid bits.

![suid](/img/anonymous/suid.png)

/usr/bin/env stood out to me so I looked it up on [gtfobins](https://gtfobins.github.io/gtfobins/env/)

![envsuid](/img/anonymous/envsuid.png)

Running /usr/bin/env /bin/sh -p gave me root.

![root flag](/img/anonymous/rootflag.png)

Question: flag.txt Answer: 4d930091c31a622a7ed10f27999af363
