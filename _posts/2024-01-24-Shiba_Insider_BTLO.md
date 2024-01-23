---
layout: post
title: Shiba Insider 
date: 2024-01-24 2:00 +0700
categories: [BTLO, Digital Forensics]
tags: [Wireshark, CyberChef, Steghide, Command Line, Exiftool, Blue Team, btlo, forensics]     # TAG names should always be lowercase
img_path: '/assets/BTLO/Shiba_Insider'
image: 
  path: logo.png
--- 

### What is the response message obtained from the PCAP file? (1 points)

We will `Follow -> TCP Stream`

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/7fbecee3-7bd5-40eb-b7ee-321a7613bd50)

`use your own password`

### What is the password of the ZIP file? (1 points)

Looking at it we will see a `Base 64` encoded string

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/4b308071-7011-42ea-9c35-8b59ae54a09f)

Use [CyberChef](https://gchq.github.io/CyberChef/) to decode

> If you don't know what the string is encoded from, you can use `Magic` from the `Operations` tab

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/839f44b0-38e2-42d9-9c5d-7cd194b596b5)

`redforever`

### Will more passwords be required? (1 points)

Before answering this question, try unzipping the file

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/96796ace-471f-4273-afc0-1350768c6991)

It's just that there's no need for more .-.

`No`

### What is the name of a widely-used tool that can be used to obtain file information? (1 points)

Definitely `Exiftool`

### What is the name and value of the interesting information obtained from the image file metadata? (1 points)

Isn't that interesting lol @~@
> It took me a while

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/8a82e296-098b-47d1-b8c7-f40e29305837)

`Technique:Steganography`

### Based on the answer from the previous question, what tool needs to be used to retrieve the information hidden in the file? (1 points)

Yes, it's already available

`Steghide`

### Enter the ID retrieved. (1 points)

As said there is no need to add a password

`steghide extract -sf ssdog1.jpeg`

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/35bea95b-d02b-4280-b66f-840f97551d87)

`0726ba878ea47de571777a`

### What is the profile name of the attacker? (3 points)

oh, the question doesn't mention 'user' or 'btlo', I tried searching and it led to someone else's answer :)))
> I didn't expect it

`https://blueteamlabs.online/home/user/0726ba878ea47de571777a`

`bluetiger`

goodbye, thank you for reading until now //~//








