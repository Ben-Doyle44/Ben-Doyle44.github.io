---
title: "Boilerctf"
description: Writeup of the boiler ctf from TryHackme, difficulty medium.
slug: "boilerctf"
date: 2022-10-09T14:11:21-04:00
draft: false
categories:
    - TryHackMe
tags:
    - Linux
    - Web-Exploitation
    - Medium
---

Started with a simple nmap scan.

![nmap output](/img/boilerctf/nmapscan.png)

It shows 4 ports open, 21(ftp), 80(http), 10000(http) and 55007(ssh).

Then I logged onto the anonymous ftp service and grabbed the .info.txt file on it.

![ftp login](/img/boilerctf/ftplogin.png)

Question: File extension after anon login Answer: .txt

Question: What is on the highest port? Answer: ssh

Question: What's running on port 10000? Answer: webmin

Question: Can you exploit the service running on that port? (yay/nay answer) Answer: nay

I then read the .info.txt file.

![ftp file](/img/boilerctf/ftpfile.png)

Using [quipquip](https://quipqiup.com/) I solved it getting "Just wanted to see if you find it. Pop. Remember: Enumeration is the key!".

Seems to be pretty useless so I then used gobuster to enumerate the first http service, port 80.

![gobuster output](/img/boilerctf/gobuster.png)

I went to robots.txt to see what was disallowed. All the urls showed did not exist. It did however, have an encoded string at the bottom.

![robots.txt](/img/boilerctf/robots.png)

"079 084 108 105 077 068 089 050 077 071 078 107 079 084 086 104 090 071 086 104 077 122 073 051 089 122 085 048 077 084 103 121 089 109 070 104 078 084 069 049 079 068 081 075"

It's ASCII and you have to convert it to base64 and then to md5. The result you get is "kidding.". It turns out /robots.txt is just a rabbit hole

The directory enumaration wasn't useless though, as both /manual and /joomla are interesting. I looked at /joomla first.

![joomla homepage](/img/boilerctf/joomlacms.png)

Question: What's CMS can you access? Answer: joomla

I moved onto enumerating /joomla using gobuster again.

![gobuster output2](/img/boilerctf/gobuster2.png)

The only one of any interest was /joomla/_test

![/_test](/img/boilerctf/_test.png)

I did a search for sar2html and found a [remote code execution on exploitdb](https://www.exploit-db.com/exploits/47204)

![exploit](/img/boilerctf/exploit.png)

I tested "http://10.10.168.219/joomla/_test/index.php?plot=;ls" to see if this rce worked as shown here:

![rce check](/img/boilerctf/rcecheck.png)

Question: The interesting file name in the folder? Answer: log.txt

So then I did "http://10.10.168.219/joomla/_test/index.php?plot=;cat%20log.txt" to see what was in log.txt, it worked. I got the ssh username: basterd and password: superduperp@$$.

I logged into the user basterd and checked if they could run anything as root, they could not. There was also no flag so I knew I needed to get access to another user to get the user flag.

![ssh login](/img/boilerctf/sshlogin.png)

I saw the backup.sh and was interested in that so I outputted it to the terminal. It showed me the username: stoner and password: superduperp@$$no1knows

![backup.sh](/img/boilerctf/backup.png)

Question: Where was the other users pass stored(no extension, just the name)? Answer: backup

I didn't see anything else of use so I moved onto the stoner user.

![stoner login](/img/boilerctf/stonerlogin.png)

I got the user flag in stoner's home directory.

![user flag](/img/boilerctf/userflag.png)

Question: user.txt Answer: You made it till here, well done.

I ran sudo -l to check what I could now run.

![sudo -l](/img/boilerctf/sudo-l.png)

Nothing of use there so I looked at what executables are of the SUID bit set.

![suid](/img/boilerctf/suid.png)

The only one of interest is "find".

I searched for find on [gtfobins](https://gtfobins.github.io/gtfobins/find/)

![gtfobins](/img/boilerctf/gtfobins.png)

![root](/img/boilerctf/root.png)

Question: What did you exploit to get the privileged user? Answer: find

Then I output root.txt to the terminal.

![root flag](/img/boilerctf/rootflag.png)

Question: root.txt Answer: It wasn't that hard, was it?


