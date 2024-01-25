---
layout: post
title: ILOVEYOU 
date: 2024-01-25 9:00 +0700
categories: [BTLO, Reverse Engineering]
tags: [Text Editor, Regshot, Blue Team, btlo, Reverse, ILOVEYOU]     # TAG names should always be lowercase
img_path: '/assets/BTLO/ILOVEYOU'
image: 
  path: logo.png
--- 

Don't forget to read the notification .-. 
> I switched to using the Kali O.S so...god

Before starting to revert it, let's learn about it

ILOVEYOU, sometimes referred to as the Love Bug or Loveletter, was a computer worm that infected over ten million Windows personal computers on and after May 5, 2000. It started spreading as an email message with the subject line `ILOVEYOU` and the attachment `LOVE-LETTER-FOR-YOU.TXT.vbs`. At the time, Windows computers often hid the latter file extension (`VBS`, a type of interpreted file) by default because it is an extension for a file type that Windows knows, leading unwitting users to think it was a normal text file.

The malware was created by Onel de Guzman, a then-24-year-old resident of Manila, Philippines.
>Wait... just 24 years old @~@ Whut the fvck?

I was so shocked that I just let it go and got to work

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/d6c696d6-75e5-4095-9c61-99194c710838)

### What is the text present as part of email when the victim received this malware? (1 points)

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/90fecaf9-6468-4c77-8274-62dd991fcb8c)

`kindly check the attached LOVELETTER coming from me`

### What is the domain name that was added as the browser's homepage? (1 points)

see it a lot 

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/533763f7-851c-4fac-b6cd-39da65aedb4a)

`http://www.skyinet.net/`

### The malware replicated itself into 3 locations, what are they? (1 points)

:)))

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/924c667f-b24c-414a-a30c-b46cd318066b)

`C:\Windows\System32\MSKernel32.vbs, C:\Windows\System32\LOVE-LETTER-FOR-YOU.TXT.vbs, C:\Windows\Win32DLL.vbs` 

### What is the name of the file that looks for the filesystem? (1 points)

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/fab8f8af-e83c-4a09-a2e3-b65d1ad89eaf)

`WinFAT32.exe`

### Which file extensions, beginning with m, does this virus target? (1 points)

wow, so this is what it's aiming for

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/0483262e-f854-4191-a554-59cfe3365af7)

`mp3, mp2`

### What is the name of the file generated when the malware identifies any Internet Relay Chat service? (1 points)

Just looking at it, you already know which file it is .-.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/d4374285-5ddf-4b01-be7c-011e98eaa3e2)

`script.ini`

### What is the name of the password stealing trojan that is downloaded by the malware? (1 points)

it makes meeeeeee so difficult :)

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/99f86b05-e198-4b65-9638-587879d2b025)

`barok`

### What is the name of the email service that is targeted by the malware? (1 points)

As I understand it, this code works on `Registry` ( `regedit.RegWrite` and `regedit.Regread`)

Run MAPI ( Messaging Application Programming Interface) and refer to (`"HKEY_CURRENT_USER\Software\Microsoft\WAB\`) but... it's so outdated compared to now 

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/85c736c0-76f0-4d52-8a54-f83ad5442c2e)

`HKEY_CURRENT_USER\Software\Microsoft\WAB\`

### What is the registry entry responsible for reading the contacts of the logged in email account? (1 points)

yayy, so easy

`outlook`

### What is the value that is stored in the registry to remember that an email was already sent to a user? (1 points)

`1`

It's almost noon so I'm going to get some food

goodbye, thank you for reading until now //~//

