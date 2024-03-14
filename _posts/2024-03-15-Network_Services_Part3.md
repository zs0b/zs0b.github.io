---
layout: post
title: Network Exploitation Basics Part 3.9
date: 2024-03-15 14:00 +0700
categories: [TryHackMe, Complete Beginner]
tags: [Blue Team, Complete Beginner]     # TAG names should always be lowercase
img_path: '/assets/THM'
image: 
  path: logo.png
--- 

## Enumerating FTP

### Lets Get Started

Before we begin, make sure to deploy the room and give it some time to boot. Please be aware, this can take up to five minutes so be patient!

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/1f512706-0b9e-4422-8c6c-f2c265062e44)

### Enumeration

By now, I don't think I need to explain any further how enumeration is key when attacking network services and protocols. You should, by now, have enough experience with nmap to be able to port scan effectively. If you get stuck using any tool- you can always use `tool [-h / -help / --help]` to find out more about it's function and syntax. Equally, man pages are extremely useful for this purpose. They can be reached using `man [tool]`

### Method

We're going to be exploiting an anonymous FTP login, to see what files we can access- and if they contain any information that might allow us to pop a shell on the system. This is a common pathway in CTF challenges, and mimics a real-life careless implementation of FTP servers.

### Resources

As we're going to be logging in to an FTP server, we will need to make sure an FTP client is installed on the system. There should be one installed by default on most Linux operating systems, such as Kali or Parrot OS. You can test if there is one by typing `ftp` into the console. If you're brought to a prompt that says: `ftp>`, then you have a working FTP client on your system. If not, it's a simple matter of using `sudo apt install ftp` to install one.

### Alternative Enumeration Methods
It's worth noting  that some vulnerable versions of in.ftpd and some other FTP server variants return different responses to the "cwd" command for home directories which exist and those that don’t. This can be exploited because you can issue cwd commands before authentication, and if there's a home directory- there is more than likely a user account to go with it. While this bug is found mainly within legacy systems, it's worth knowing about, as a way to exploit FTP.

This vulnerability is documented at: [https://www.exploit-db.com/exploits/20745](https://www.exploit-db.com/exploits/20745)

Now we understand our toolbox, let's do this.              

Run an **nmap** scan of your choice.

### How many **ports** are open on the target machine? 

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/16896caa-d154-4ad4-ad99-c941d884c011)

`1`

### What **port** is ftp running on?

`21`

### What **variant** of FTP is running on it?  

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/f01bef37-195d-4249-91f0-578c6f67e06d)

`vsFTPd`

Great, now we know what type of FTP server we're dealing with we can check to see if we are able to login anonymously to the FTP server. We can do this using by typing "*ftp [IP]*"  into the console, and entering `anonymous`, and no password when prompted.

### What is the name of the file in the anonymous FTP directory?

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/397394f8-1e9f-42a9-9022-99d08297f57f)

`PUBLIC_NOTICE.txt`

### What do we think a possible username could be?

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/4a3711a3-4a24-46d5-8c38-09f62c1da1da)

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/78a8b04a-e21a-4d9b-9e3b-f6cd6a711efc)

`mike`

## Exploiting FTP

### Types of FTP Exploit

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/6f90cd37-77e7-4ced-8858-8463e4a68e1e)

Similarly to Telnet, when using FTP both the command and data channels are unencrypted. Any data sent over these channels can be intercepted and read.

With data from FTP being sent in plaintext, if a man-in-the-middle attack took place an attacker could reveal anything sent through this protocol (such as passwords). An article written by [JSCape](https://www.jscape.com/blog/countering-packet-sniffers-using-encrypted-ftp) demonstrates and explains this process using ARP-Poisoning to trick a victim into sending sensitive information to an attacker, rather than a legitimate source.

When looking at an FTP server from the position we find ourselves in for this machine, an avenue we can exploit is weak or default password configurations.

### Method Breakdown

So, from our enumeration stage, we know:

  - There is an FTP server running on this machine

  - We have a possible username

Using this information, let's try and **bruteforce** the password of the FTP Server.

### Hydra

Hydra is a very fast online password cracking tool, which can perform rapid dictionary attacks against more than 50 Protocols, including Telnet, RDP, SSH, FTP, HTTP, HTTPS, SMB, several databases and much more. Hydra comes by default on both Parrot and Kali, however if you need it, you can find the GitHub [here](https://github.com/vanhauser-thc/thc-hydra).

The syntax for the command we're going to use to find the passwords is this:

`hydra -t 4 -l dale -P /usr/share/wordlists/rockyou.txt -vV 10.10.10.6 ftp`

Let's break it down:

SECTION             FUNCTION

hydra                   Runs the hydra tool

-t 4                    Number of parallel connections per target

-l `[user]`               Points to the user who's account you're trying to compromise

-P `[path to dictionary]` Points to the file containing the list of possible passwords

-vV                     Sets verbose mode to very verbose, shows the login+pass combination for each attempt

`[machine IP]`            The IP address of the target machine

ftp / protocol          Sets the protocol

Let's crack some passwords!

### What is the password for the user "mike"?

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/458c3ac5-463a-4940-b86c-c0e51c5b2ad5)

`password`

Bingo! Now, let's connect to the FTP server as this user using `ftp [IP]` and entering the credentials when prompted

### What is ftp.txt?

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/09888b0e-768d-4edd-a53d-c833b7bf0e64)

`THM{y0u_g0t_th3_ftp_fl4g}`

## Expanding Your Knowledge

### Further Learning

There is no checklist of things to learn until you've officially learnt everything you can. There will always be things that surprise us all, especially in the sometimes abstract logical problems of capture the flag challenges. But, as with anything, practice makes perfect. We can all look back on the things we've learnt after completing something challenging and I hope you feel the same about this room.

### Reading

Here's some things that might be useful to read after completing this room, if it interests you:

  - [https://medium.com/@gregIT/exploiting-simple-network-services-in-ctfs-ec8735be5eef](https://gregit.medium.com/exploiting-simple-network-services-in-ctfs-ec8735be5eef)

  - [https://attack.mitre.org/techniques/T1210/](https://attack.mitre.org/techniques/T1210/)

  - [https://www.nextgov.com/cybersecurity/2019/10/nsa-warns-vulnerabilities-multiple-vpn-services/160456/](https://www.nextgov.com/cybersecurity/2019/10/nsa-warns-vulnerabilities-multiple-vpn-services/160456/)

goodbye, thank you for reading until now //~//




























