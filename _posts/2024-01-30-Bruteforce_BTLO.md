--- 
layout: post
title: Bruteforce
date: 2024-01-30 8:00 +0700
categories: [BTLO, Incident Response]
tags: [Grep, Text Editor, Excel, Blue Team, btlo, Bruteforce]     # TAG names should always be lowercase
img_path: '/assets/BTLO'
image: 
  path: logo.png
--- 

### Question 1) How many Audit Failure events are there? (Format: Count of Events) (3 points)

For the first question, we just need to search for `Audit Failure` to get the answer ~

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/403953a5-a5e0-4f45-82d7-fc24a584739c)

You can also open it with `Mousepad` search for ease

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/da6fdef7-6a48-4474-9f34-5127fae9d98e)


`3103`

### Question 2) What is the username of the local account that is being targeted? (Format: Username) (2 points)

Because I switched from a Windows O.S to a Kali O.S, I may not be familiar with it, and... I struggled for a while to look at the `.evtx` file .-.
> But... it's so hard to see, my eyes TT

```
evtx_dump.py BTLO_Bruteforce_Challenge.evtx

```

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/17e11b5d-66d2-46a2-877e-9450c483ce67)

`administrator` 

### Question 3) What is the failure reason related to the Audit Failure logs? (Format: String) (3 points)

It's pretty easy~

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/01ff841e-ae4f-4e3b-9631-3fda482fb6a5)

`Unknown user name or bad password`

### Question 4) What is the Windows Event ID associated with these logon failures? (Format: ID) (3 points)

So... you can see it when you look at it

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/e5c486a7-52c2-4509-9bf5-fd075a82031b)

`4625` 

### Question 5) What is the source IP conducting this attack? (Format: X.X.X.X) (3 points)

Hmmm...

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/60f3abc7-360c-4222-aade-9daf2621dbac)

`113.161.192.227`

### Question 6) What country is this IP address associated with? (Format: Country) (3 points)

ehhhh... located in my country ah =)))

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/0fc130e7-1a74-415a-9d0c-9857de816816)

`Vietnam`

### Question 7) What is the range of source ports that were used by the attacker to make these login requests? (LowestPort-HighestPort - Ex: 100-541) (3 points)

I roughed this question by searching `Source Port` and finding it was quite easy because it runs in order :))
> let's try it

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/37db576c-824a-49b6-a7c6-889ca694954b)

`49162–65534`

goodbye, thank you for reading until now //~//









