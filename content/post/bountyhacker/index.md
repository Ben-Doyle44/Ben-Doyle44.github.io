---
title: "Bountyhacker"
description: Writeup of bounty hacker from TryHackme, difficulty easy.
slug: "bountyhacker"
date: 2022-10-09T10:24:24-04:00
draft: false
categories:
    - TryHackMe
tags:
    - Linux
    - Easy
---

I ran an nmap scan to know what ports are open and what is using them.

![nmap output](/img/bountyhacker/nmapscan.png)

It shows 3 ports open, 21(ftp), 22(ssh) and 80(http).

Anonymous login is allowed on the ftp service so I looked at that first. 2 text files were present so I grabbed them both.

![ftp login](/img/bountyhacker/ftplogin.png)

The file locks.txt seems to be a list of password and task.txt is a message to somebody called "lin", what I assumed to be a username.

![files output](/img/bountyhacker/filesoutput.png)

Question: Who wrote the task list? Answer: lin 

I used hydra on the ssh port with locks.txt as the wordlist and lin as the username.

![hydra](/img/bountyhacker/hydra.png)

It worked giving us the password "RedDr4gonSynd1cat3"

Question: What service can you bruteforce with the text file found? Answer: ssh

Question: What is the users password? Answer: RedDr4gonSynd1cat3

I logged in and read the user.txt giving me the user flag.

![user flag](/img/bountyhacker/userflag.png)

Question: user.txt Answer: THM{CR1M3_SyNd1C4T3}


I used sudo -l to look at what could run as root.

![sudo -l](/img/bountyhacker/sudo-l.png)

I saw tar could run as root with no password so I looked it up on [gtfobins](https://gtfobins.github.io/gtfobins/tar/).

![tar sudo](/img/bountyhacker/tarsudo.png)

![root](/img/bountyhacker/root.png)

Then I read root.txt giving me the root flag.

![root flag](/img/bountyhacker/rootflag.png)

Question: root.txt Answer: THM{80UN7Y_h4cK3r}
