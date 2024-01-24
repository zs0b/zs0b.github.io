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

```
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

```
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

```
sudo gzip -d /usr/share/wordlists/rockyou.txt.gz
sudo apt install fcrackzip
```
Yeh, that's all there is, now let's get started as a hacker :)))

```
sudo fcrackzip -D -p /usr/share/wordlists/rockyou.txt f0000240_brown.zip

```

Password: `garfield`

Using that to decompress we have

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/ce7e525b-200c-48f6-a1f2-4ab56fe69cf7)

Right, now that we have the file, let's try steghide -.-

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/3270e694-ee35-41e1-a8a7-7b50280b9501)

Ummm,...

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/c7c4af8c-8396-4c0b-8f22-4098bfcc080f)

After a while of trying again and again, I knew it was encoded from `base 58`

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/6661a385-2364-462f-aec2-db0371353ff9)

but... it must have been reversed -_-

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/8cfb89c9-9738-4335-835e-8ef5d741e0f5)

`00:10:51`

### What are the supposed coordinates for the deal? (4 points)

And then, now go back and continue with `white.wav`, ignoring all the others =]
> Tags mentioning `Audio Software`, I like `Audacity` because I like it @.@

As usual when testing with `Audacity` switch to `Spectrogram` and miracles will happen

I bet this is a GPS map, trust me!!
> I'm never wrong once, I'm wrong a lot XD

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/ccd8cf77-5a89-4558-9f49-48aebb3a81eb)

`51.505278 0.055278`

Go to `GG Map` and enter like me and boom


![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/2528225f-2457-4d22-84ea-c2dddd412a41)

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/a3b66f98-c88a-4575-b8f2-b450b1e066dd)

`London City Airport`

goodbye, thank you for reading until now //~//



























