--- 
layout: post
title: Paranoid
date: 2024-01-30 8:00 +0700
categories: [BTLO, Incident Response]
tags: [AUReport, Linux CLI, Blue Team, btlo, auditd]     # TAG names should always be lowercase
img_path: '/assets/BTLO'
image: 
  path: logo.png
--- 

Hi :))

Before jumping in, look at the tag and file of the article

`Audit.log` file, go to google search, there is a tool for it
> yeh, there is an auditd tool lol XD

```
sudo apt-get update
sudo apt-get -y install auditd
```

Remember to log in to root, comrades -..-
> After downloading, there are many tools, please explain here hehe

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/9d0d1f5a-2465-403d-a5c8-47f8362bbb17)

After reading the instructions, to get an overview, use the command line below
>  -if,--input <Input File name>: use this file as input

>  --summary: sorted totals for main object in report

```
aureport --summary -if audit.log 

```

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/6d91bfcb-725f-4c64-938c-1d749700594a)

okee, swim to solve this challenge

### What account was compromised? (2 points)
  
To know which account is compromised, look at the `authentication report`
> -au,--auth: Authentication report

```
aureport -au -if audit.log

```

According to the report, left to right will be `date, time, acct, host, term, path, success, event`
> Wow maybe that's it I'm a newbie (.-.) , sorry if it's wrong hehe

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/d7a0f374-1579-4a09-9b33-c56acbfcde99)

`btlo`

### What attack type was used to gain initial access? (2 points)

From the picture I took above, it could only be a brute force attack =)) returning too many events in 1 second

`brute force`

### What is the attacker's IP address? (2 points)

I listed it above as the host column 

`192.168.4.155`

### What tool was used to perform system enumeration? (2 points)

If the hacker already has permission, then see what he typed on the console
> --tty: Report about tty keystrokes

```
aureport --tty -if audit.log

```

From the image , the hacker `wget` the command file `linpeas.sh` from his machine with port `8000`

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/c6508e62-5ee0-4dd7-ab6f-765acdfaea06)

After researching on Google, LinPeas is a command file that searches for paths to elevate privileges on Linux/Unix*/MacOS hosts.

```
lsb_release -a

```

The command lsb_release -a is used to display version information and description of a Linux distribution based on the LSB (Linux Standard Base) standard.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/48ee62e1-c574-4bda-94a7-0f72ae8123d4)

Then he `wget` file `evil.tar.gz` from his computer, the attacker executed the command `make` ... wait pause here, what is the `make` command used for
> There is a document for you to clearly understand the [make command](https://www.ibm.com/docs/en/aix/7.2?topic=concepts-make-command)

> But to put it simply, it will use information from the description file, which the program creator has completed, which helps eliminate repetition and speed up compilation as well as save time.

then he executed `./evil` with argument `0`, I don't really understand here @~@, and then he erased his tracks and escaped

### What is the name of the binary and pid used to gain root? (3 points)

After analyzing above, we know that there is a command file that was executed as `evil`, ok so let's list the ids of the executing processes.
> -p,--pid: Pid report

> I forgot, there will be a lot of them and not very related to what we are looking for, let's filter them with `grep 'evil'`

```
aureport -p -if audit.log | grep 'evil'

```

From left to right we have `date, pid, path, UID, GID, event ID`

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/2ac907cc-298f-4288-a89e-77b72e267bb9)

`evil, 829992` 

### What CVE was exploited to gain root access? (Do your research!) (3 points)

hehe too easy

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/bb42068a-d5e7-4332-9398-2f7b30865353)

`CVE-2021-3560`

boom!, wrong right =))) I also thought it would be easy to eat kkk
> keep searching

try it and see if it's right

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/bf624bbb-080b-4698-b018-1fa8660efd6e)

`CVE-2021-3156`

ohhhhhhhhhhhhhhh~ that's righttttttt
> Here is it [CVE-2021-3156](https://www.alibabacloud.com/help/en/ecs/product-overview/vulnerability-announcement-or-linux-sudo-permission-vulnerability)

### What type of vulnerability is this? (3 points)

right ~ 

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/61b5223e-6f7a-4fc5-9404-fadf14177d41)

`Heap-Based Buffer Overflow`

### What file was exfiltrated once root was gained? (3 points)

I've been confused about this all day :))
> And after looking back, I realized I missed it :_) 

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/9c182e97-22f6-4f75-949c-a8a8cc6b44cf)

`/etc/shadow`

ok okay I'm tired TT

goodbye, thank you for reading until now //~//

![Alt Text](https://media.giphy.com/media/nmBKiNb7h3tIv3BO8D/giphy.gif?cid=790b761183ingxqbsyfgt5y2bt92cadaqbpp5jl7drplf3a4&ep=v1_gifs_search&rid=giphy.gif&ct=g)
















