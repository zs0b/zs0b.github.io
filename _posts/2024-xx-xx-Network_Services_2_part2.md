---
layout: post
title: Network Services 2 part 2
date: 2024-03-17 9:00 +0700
categories: [TryHackMe, Complete Beginner]
tags: [Blue Team, Complete Beginner]     # TAG names should always be lowercase
img_path: '/assets/THM'
image: 
  path: logo.png
--- 

## Understanding SMTP

### What is SMTP?

SMTP stands for "Simple Mail Transfer Protocol". It is utilised to handle the sending of emails. In order to support email services, a protocol pair is required, comprising of SMTP and POP/IMAP. Together they allow the user to send outgoing mail and retrieve incoming mail, respectively.

The SMTP server performs three basic functions:

- It verifies who is sending emails through the SMTP server.

- It sends the outgoing mail

- If the outgoing mail can't be delivered it sends the message back to the sender

Most people will have encountered SMTP when configuring a new email address on some third-party email clients, such as Thunderbird; as when you configure a new email client, you will need to configure the SMTP server configuration in order to send outgoing emails.

### POP and IMAP

POP, or "Post Office Protocol" and IMAP, "Internet Message Access Protocol" are both email protocols who are responsible for the transfer of email between a client and a mail server. The main differences is in POP's more simplistic approach of downloading the inbox from the mail server, to the client. Where IMAP will synchronise the current inbox, with new mail on the server, downloading anything new. This means that changes to the inbox made on one computer, over IMAP, will persist if you then synchronise the inbox from another computer. The POP/IMAP server is responsible for fulfiling this process.

### How does SMTP work?

Email delivery functions much the same as the physical mail delivery system. The user will supply the email (a letter) and a service (the postal delivery service), and through a series of steps- will deliver it to the recipients inbox (postbox). The role of the SMTP server in this service, is to act as the sorting office, the email (letter) is picked up and sent to this server, which then directs it to the recipient.
We can map the journey of an email from your computer to the recipient’s like this:

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/9d5efbe0-4167-43f2-a3dc-395d08f73ce9)

1. The mail user agent, which is either your email client or an external program. connects to the SMTP server of your domain, e.g. smtp.google.com. This initiates the SMTP handshake. This connection works over the SMTP port- which is usually 25. Once these connections have been made and validated, the SMTP session starts.

2. The process of sending mail can now begin. The client first submits the sender, and recipient's email address- the body of the email and any attachments, to the server.

3. The SMTP server then checks whether the domain name of the recipient and the sender is the same.

4. The SMTP server of the sender will make a connection to the recipient's SMTP server before relaying the email. If the recipient's server can't be accessed, or is not available- the Email gets put into an SMTP queue.

5. Then, the recipient's SMTP server will verify the incoming email. It does this by checking if the domain and user name have been recognised. The server will then forward the email to the POP or IMAP server, as shown in the diagram above.

6. The E-Mail will then show up in the recipient's inbox.

This is a very simplified version of the process, and there are a lot of sub-protocols, communications and details that haven't been included. If you're looking to learn more about this topic, this is a really friendly to read breakdown of the finer technical details- I actually used it to write this breakdown:

[https://computer.howstuffworks.com/e-mail-messaging/email3.htm](https://computer.howstuffworks.com/e-mail-messaging/email3.htm)

### What runs SMTP?

SMTP Server software is readily available on Windows server platforms, with many other variants of SMTP being available to run on Linux.

### More Information:

Here is a resource that explain the technical implementation, and working of, SMTP in more detail than I have covered here.

[https://www.afternerd.com/blog/smtp/](https://www.afternerd.com/blog/smtp/)

### What does SMTP stand for?

`Simple Mail Transfer Protocol`

### What does SMTP handle the sending of? (answer in plural)

`emails`

### What is the first step in the SMTP process?

`SMTP handshake`

### What is the default SMTP port?

`25`

### Where does the SMTP server send the email if the recipient's server is not available?

`smtp queue`

### On what server does the Email ultimately end up on?

`POP/IMAP`

### Can a Linux machine run an SMTP server? (Y/N)

`Y`

### Can a Windows machine run an SMTP server? (Y/N)

`Y`

## Enumerating SMTP

### Lets Get Started

Before we begin, make sure to deploy the room and give it some time to boot. Please be aware, this can take up to five minutes so be patient!

### Enumerating Server Details

Poorly configured or vulnerable mail servers can often provide an initial foothold into a network, but prior to launching an attack, we want to fingerprint the server to make our targeting as precise as possible. We're going to use the "smtp_version" module in MetaSploit to do this. As its name implies, it will scan a range of IP addresses and determine the version of any mail servers it encounters.

### Enumerating Users from SMTP

The SMTP service has two internal commands that allow the enumeration of users: VRFY (confirming the names of valid users) and EXPN (which reveals the actual address of user’s aliases and lists of e-mail (mailing lists). Using these SMTP commands, we can reveal a list of valid users

We can do this manually, over a telnet connection- however Metasploit comes to the rescue again, providing a handy module appropriately called "smtp_enum" that will do the legwork for us! Using the module is a simple matter of feeding it a host or range of hosts to scan and a wordlist containing usernames to enumerate.

### Requirements
As we're going to be using Metasploit for this, it's important that you have Metasploit installed. It is by default on both Kali Linux and Parrot OS; however, it's always worth doing a quick update to make sure that you're on the latest version before launching any attacks. You can do this with a simple "sudo apt update", and accompanying upgrade- if any are required.

### Alternatives

It's worth noting that this enumeration technique will work for the majority of SMTP configurations; however there are other, non-metasploit tools such as smtp-user-enum that work even better for enumerating OS-level user accounts on Solaris via the SMTP service. Enumeration is performed by inspecting the responses to VRFY, EXPN, and RCPT TO commands.

This technique could be adapted in future to work against other vulnerable SMTP daemons, but this hasn’t been done as of the time of writing. It's an alternative that's worth keeping in mind if you're trying to distance yourself from using Metasploit e.g. in preparation for OSCP.

Now we've covered the theory. Let's get going!

### First, lets run a port scan against the target machine, same as last time. What port is SMTP running on?

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/eccb6791-ab2f-4967-a12a-171a2fba1ec2)

`25`

### Okay, now we know what port we should be targeting, let's start up Metasploit. What command do we use to do this?

If you would like some more help or practice using Metasploit, TryHackMe has a module on Metasploit that you can check out here:

[https://tryhackme.com/module/metasploit](https://tryhackme.com/module/metasploit)

`msfconsole`

### Let's search for the module "smtp_version", what's it's full module name?

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/00dd4fb7-476c-4534-ad04-0a12f0dc6e9f)

`auxiliary/scanner/smtp/smtp_version`

### Great, now- select the module and list the options. How do we do this?

`options`

### Have a look through the options, does everything seem correct? What is the option we need to set?

`RHOSTS`

### Set that to the correct value for your target machine. Then run the exploit. What's the system mail name?

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/591d2392-186c-4564-9fcd-2d7c451a720a)

`polosmtp.home`

### What Mail Transfer Agent (MTA) is running the SMTP server? This will require some external research.

`postfix`

### Good! We've now got a good amount of information on the target system to move onto the next stage. Let's search for the module "smtp_enum", what's it's full module name?

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/1023258a-f166-463f-9d89-b2542da0eaef)

`auxiliary/scanner/smtp/smtp_enum`

We're going to be using the "top-usernames-shortlist.txt" wordlist from the Usernames subsection of seclists (/usr/share/wordlists/SecLists/Usernames if you have it installed).

Seclists is an amazing collection of wordlists. If you're running Kali or Parrot you can install seclists with: "sudo apt install seclists" Alternatively, you can download the repository from [here](https://github.com/danielmiessler/SecLists).

### What option do we need to set to the wordlist's path?

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/824e7e13-bdfc-4c2e-a56e-342cdd775bc4)

`USER_FILE`

### Once we've set this option, what is the other essential paramater we need to set?

`RHOSTS`

### Okay! Now that's finished, what username is returned?

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/0b9c12ae-299c-47f1-b0bd-002a8db622fe)

`administrator`

## Exploiting SMTP

### What do we know?

Okay, at the end of our Enumeration section we have a few vital pieces of information:

1. A user account name

2. The type of SMTP server and Operating System running.

We know from our port scan, that the only other open port on this machine is an SSH login. We're going to use this information to try and **bruteforce** the password of the SSH login for our user using Hydra.

### Preparation

It's advisable that you exit Metasploit to continue the exploitation of this section of the room. Secondly, it's useful to keep a note of the information you gathered during the enumeration stage, to aid in the exploitation.

### Hydra

There is a wide array of customisability when it comes to using Hydra, and it allows for adaptive password attacks against of many different services, including SSH. Hydra comes by default on both Parrot and Kali, however if you need it, you can find the GitHub [here](https://github.com/vanhauser-thc/thc-hydra).

Hydra uses dictionary attacks primarily, both Kali Linux and Parrot OS have many different wordlists in the " */usr/share/wordlists* " directory- if you'd like to browse and find a different wordlists to the widely used "rockyou.txt". Likewise I recommend checking out SecLists for a wider array of other wordlists that are extremely useful for all sorts of purposes, other than just password cracking. E.g. subdomain enumeration
The syntax for the command we're going to use to find the passwords is this:

`hydra -t 16 -l USERNAME -P /usr/share/wordlists/rockyou.txt -vV 10.10.69.214 ssh`

Let's break it down:

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/a4ece48d-490d-4b0a-ada5-8489023562d7)

Looks like we're ready to rock n roll!

### What is the password of the user we found during our enumeration stage?

`hydra -l administrator -P /usr/share/wordlists/rockyou.txt 10.10.69.214 ssh`

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/0c486c79-6559-4d4c-b3a9-2d8b4c227415)

`alejandro`

### Great! Now, let's SSH into the server as the user, what is contents of smtp.txt

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/641a2c05-de58-44f6-abb4-df0d8914a924)

`THM{who_knew_email_servers_were_c00l?}`



















