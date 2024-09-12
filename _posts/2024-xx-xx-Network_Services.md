---
layout: post
title: Network Exploitation Basics Part 3
date: 2024-03-13 22:00 +0700
categories: [TryHackMe, Complete Beginner]
tags: [Blue Team, Complete Beginner]     # TAG names should always be lowercase
img_path: '/assets/THM'
image: 
  path: logo.png
--- 

## Understanding SMB

### What is SMB?

SMB - Server Message Block Protocol - is a client-server communication protocol used for sharing access to files, printers, serial ports and other resources on a network. [source](https://www.techtarget.com/searchnetworking/definition/Server-Message-Block-Protocol)

Servers make file systems and other resources (printers, named pipes, APIs) available to clients on the network. Client computers may have their own hard disks, but they also want access to the shared file systems and printers on the servers.

The SMB protocol is known as a response-request protocol, meaning that it transmits multiple messages between the client and server to establish a connection. Clients connect to servers using TCP/IP (actually NetBIOS over TCP/IP as specified in RFC1001 and RFC1002), NetBEUI or IPX/SPX.

### How does SMB work?

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/48c10590-c32e-4aaf-a9ec-7e4df9e341e3)

Once they have established a connection, clients can then send commands (SMBs) to the server that allow them to access shares, open files, read and write files, and generally do all the sort of things that you want to do with a file system. However, in the case of SMB, these things are done over the network.

### What runs SMB?

Microsoft Windows operating systems since Windows 95 have included client and server SMB protocol support. Samba, an open source server that supports the SMB protocol, was released for Unix systems

### What does SMB stand for?    

`Server Message Block`

### What type of protocol is SMB?    

`response-request`

### What do clients connect to servers using?    

`TCP/IP`

### What systems does Samba run on?

`Unix`

## Enumerating SMB

### Lets Get Started

Before we begin, make sure to deploy the room and give it some time to boot. Please be aware, this can take up to five minutes so be patient!

### Enumeration

Enumeration is the process of gathering information on a target in order to find potential attack vectors and aid in exploitation.

This process is essential for an attack to be successful, as wasting time with exploits that either don't work or can crash the system can be a waste of energy. Enumeration can be used to gather usernames, passwords, network information, hostnames, application data, services, or any other information that may be valuable to an attacker.

### SMB

Typically, there are SMB share drives on a server that can be connected to and used to view or transfer files. SMB can often be a great starting point for an attacker looking to discover sensitive information — you'd be surprised what is sometimes included on these shares.

### Port Scanning

The first step of enumeration is to conduct a port scan, to find out as much information as you can about the services, applications, structure and operating system of the target machine.

If you haven't already looked at port scanning, I **recommend** checking out the Nmap room here

### Enum4Linux

Enum4linux is a tool used to enumerate SMB shares on both Windows and Linux systems. It is basically a wrapper around the tools in the Samba package and makes it easy to quickly extract information from the target pertaining to SMB. It's installed by default on Parrot and Kali, however if you need to install it, you can do so from the official [github](https://github.com/CiscoCXSecurity/enum4linux).

The syntax of Enum4Linux is nice and simple: `num4linux [options] ip`

**TAG**            **FUNCTION**

-U             get userlist

-M             get machine list

-N             get namelist dump (different from -U and-M)

-S             get sharelist

-P             get password policy information

-G             get group and member list

-a             all of the above (full basic enumeration)

Now we understand our enumeration tools, let's get started!

### Conduct an **nmap** scan of your choosing, How many ports are open?

`nmap -sC -sV  10.10.146.211`

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/7b3166f3-05cf-4129-8dc7-46c5e9265158)

`3`

### What ports is SMB running on?

`139/445`

### Let's get started with Enum4Linux, conduct a full basic enumeration. For starters, what is the workgroup name?    

`enum4linux -a 10.10.146.211`

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/2d1e4018-1e4f-4635-8aed-baac168f281c)

`WORKGROUP`

### What comes up as the name of the machine?        

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/65ed10a5-9d98-4c96-95fc-0d35515f3592)

`POLOSMB`

### What operating system version is running?    

`6.1`

### What share sticks out as something we might want to investigate?    

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/f4141ec0-776a-42cc-abfa-9a40b4cf2787)

## Exploiting SMB

### Types of SMB Exploit

While there are vulnerabilities such as [CVE-2017-7494](https://www.cvedetails.com/cve/CVE-2017-7494/) that can allow remote code execution by exploiting SMB, you're more likely to encounter a situation where the best way into a system is due to misconfigurations in the system. In this case, we're going to be exploiting anonymous SMB share access- a common misconfiguration that can allow us to gain information that will lead to a shell.

### Method Breakdown

So, from our enumeration stage, we know:

    - The SMB share location

    - The name of an interesting SMB share

### SMBClient

Because we're trying to access an SMB share, we need a client to access resources on servers. We will be using SMBClient because it's part of the default samba suite. While it is available by default on Kali and Parrot, if you do need to install it, you can find the documentation [here](https://www.samba.org/samba/docs/current/man-html/smbclient.1.html).

We can remotely access the SMB share using the syntax:

`smbclient //[IP]/[SHARE]`

Followed by the tags:

-U [name] : to specify the user

-p [port] : to specify the port

**Got it? Okay, let's do this!**

### What would be the correct syntax to access an SMB share called "secret" as user "suit" on a machine with the IP 10.10.10.2 on the default port?

`smbclient //10.10.10.2/secret -U suit -p 445`

### Lets see if our interesting share has been configured to allow anonymous access, I.E it doesn't require authentication to view the files. We can do this easily by:

- using the username "Anonymous"

- connecting to the share we found during the enumeration stage

- and not supplying a password.

Does the share allow anonymous access? Y/N?

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/170aef15-720b-4513-acf9-1719e1ab07eb)

`Y`

### Great! Have a look around for any interesting documents that could contain valuable information. Who can we assume this profile folder belongs to?

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/307379fe-df0f-4e3b-aa15-30d6abd64585)

`more "Working From Home Information.txt"`

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/aee06aed-93cc-41ed-9ae4-d54997559f1c)

`John Cactus`

### What service has been configured to allow him to work from home?

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/4e259043-e293-4ec4-85a8-c15addcf30a9)

`ssh`

### Okay! Now we know this, what directory on the share should we look in?

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/f56fd261-c701-474e-802a-00e618a8c169)

`.ssh`

### This directory contains authentication keys that allow a user to authenticate themselves on, and then access, a server. Which of these keys is most useful to us?

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/6d801de0-da25-4826-831f-e6998918659d)

`id_rsa`

### Download this file to your local machine, and change the permissions to "600" using "chmod 600 [file]".

Now, use the information you have already gathered to work out the username of the account. Then, use the service and key to log-in to the server.

What is the smb.txt flag?

`mget id*`

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/44122761-fa54-49a8-af80-4038243d3572)

```
cd .ssh
chmod 600 id_rsa
ssh cactus@10.10.146.211
```

>cactus from file "Working From Home Information.txt"

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/38e0b811-183c-4eb6-96f8-49aea0afc47e)

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/9bbe3e23-ba6e-4bdb-8d27-f6d8fd223210)

`THM{smb_is_fun_eh?}`

##  Understanding Telnet

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/8e4aaa29-ff34-46a8-ad0a-05d94e1d94d4)

### What is Telnet?

Telnet is an application protocol which allows you, with the use of a telnet client, to connect to and execute commands on a remote machine that's hosting a telnet server.

The telnet client will establish a connection with the server. The client will then become a virtual terminal- allowing you to interact with the remote host.

### Replacement

Telnet sends all messages in clear text and has no specific security mechanisms. Thus, in many applications and services, Telnet has been replaced by SSH in most implementations.

### How does Telnet work?

The user connects to the server by using the Telnet protocol, which means entering **"telnet"** into a command prompt. The user then executes commands on the server by using specific Telnet commands in the Telnet prompt. You can connect to a telnet server with the following syntax: **"telnet [ip] [port]"**

### What is Telnet?    

`application protocol`

### What has slowly replaced Telnet?    

`ssh`

### How would you connect to a Telnet server with the IP 10.10.10.3 on port 23?

`telnet 10.10.10.3 23`

### The lack of what, means that all Telnet communication is in plaintext?

`encryption`

goodbye, thank you for reading until now //~//

Then wait until tomorrow >.0

