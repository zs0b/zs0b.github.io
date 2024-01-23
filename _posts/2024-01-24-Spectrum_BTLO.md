---
layout: post
title: Spectrum 
date: 2024-01-24 2:00 +0700
categories: [BTLO, Digital Forensics]
tags: [Photorec, Audio Software, steghide, fcrackzip, exiftool, Blue Team, btlo, forensics]     # TAG names should always be lowercase
img_path: '/assets/BTLO/Spectrum'
image: 
  path: logo.png
--- 

### What time is the meeting happening? (4 points)

Seeing this file is quite strange, let's see what it is before jumping into it 0.<

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/6138d1bb-98e9-4165-a2a3-15cf679d37a3)

Ok, I know it is a file `disk image`

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/1e09ca92-e04a-458f-93d1-589e96d92b68)

Thread tags mentioning a tool `Photorec`

```Terminal
photorec image.dd 
```

Select `Disk image.dd` -> `Proceed`

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/4115055d-2082-4b64-95e3-1a1b13b177f0)

Select `FAT16` -> `Search`

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/8abb1791-79ab-4ab4-b31a-a5823a69d192)

Select `ext2/ext3`

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/569f289d-082e-4be1-88b4-c338ec8b3f7e)

Select `Whole` to extract the whole thing

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/96fc0eaa-ca0f-4074-8d6a-e1ab06280809)

Select the destination region to export

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/d19fbb96-cce0-4977-9df6-9a99d449b5ba)

OK, restore is done

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/3273d285-a9ad-4c1c-afc4-2c0f130a1974)

```Terminal
exiftool *.jpg
```

There are 2 things to note:
- Location:name of the challenge
- Artist:steghide password: cheese on toast
>Where is `Spectrum`?

>Password `steghide` but which file is it from?

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/9641aeb3-28c8-44e3-835e-f2da9b8b173d)

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/82cbcfd3-deb1-49d4-aed9-7a29719a7626)

I tried all .`jpg` image files and `.zip` files but it doesn't work .-.

Ignore it for now :)), if the tags mention fcrackzip, let's try it
> In the linux machine, there is a `rockyou` file available, take it out and use it

Command below to decompress and download the tool

```Terminal
sudo gzip -d /usr/share/wordlists/rockyou.txt.gz
sudo apt install fcrackzip
```
Yeh, that's all there is, now let's get started as a hacker :)))

```Terminal
sudo fcrackzip -D -p /usr/share/wordlists/rockyou.txt f0000240_brown.zip 
```
Password: `garfield`

Using that to decompress we have

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/ce7e525b-200c-48f6-a1f2-4ab56fe69cf7)















