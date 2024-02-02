---
layout: post
title: SUKANA 
date: 2024-02-02 16:00 +0700
categories: [BTLO, Investigations, INCIDENT RESPONSE]
tags: [Thunderbird, VirusTotal, Cisco Talos, Certutil, Volatility3, Volatility Workbench, Autopsy, Wireshark, Timeline Explorer, CyberChef, MFTECmd, Blue Team, btlo]     # TAG names should always be lowercase
img_path: '/assets/BTLO/Sukana'
image: 
  path: logo.png
--- 

Hello everyone, today we will go to the `investigation` part

### Q1) What is the SHA256 hash value for the malware file? (Format: SHA256)(1 points)

Open the folder `BTLO-Sukana` and see a folder `Intezer Report` 
> When investigating, we always have to look at the reports about it, right?

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/5c1fcfb1-94fd-4e0b-8f9b-de776c93fbc2)

`a879d2c1608c4b5cf801c2ab49b54b4139aa13f636fc6495fcaf940591713905` 

### Q2) What is the file size of the malware in bytes? (Format: XXXXXX)(1 points)

Right below there is the file `related-samples-5ba7730a-5bdd-425d-8676-e5207407b98a`
> Open it up and see

Did you notice that there are comments? you see `file size`
> Don't rush

Look at `SHA256`, yeh it's not the `file size` I'm looking for, kkk

See the link next to `VT` ?, look at the link and see the line `SHA256`, ok so the line `SHA256` was found in sentence 1 and just search
> [Here](https://www.virustotal.com/gui/file/a879d2c1608c4b5cf801c2ab49b54b4139aa13f636fc6495fcaf940591713905/details) 

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/dfd75d5a-42d5-4810-8816-5f10b9822144)

Here is the `file size` we need to find TT

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/6ee67a3b-b8f7-4319-b47c-1d95b4c73386)

`445485`

### Q3) Utilizing Cisco Talos, what is the “Cisco Secure Endpoint Detection Name” for the given malware? (Format: Name)(1 points)

We use [Cisco Talos](https://www.talosintelligence.com/talos_file_reputation), add `SHA256` 

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/e9139449-69f6-439b-b1b5-d1f78c189059)

`W32.Auto:a879d2.in03.Talos`

### Q4) Utilizing Cisco Talos, what is the file reputation of the given malware? (Format: String)(1 points)

The first sentences are quite simple

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/d7804afd-ac6e-4ade-a7e7-603d6cad1647)

`Malicious`

### Q5) With Windows Defender enabled, launch the malware. What is the name of the threat according to Microsoft Defender Antivirus? (Format: Name)(1 points)

Open the file I pointed to with `Thunderbird`

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/eea9c62c-4e8c-464a-b20e-498cee102396)

There is an attached file, download it

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/5a43ba59-abff-4548-bf00-b4b459bac059)

`Windows Security` immediately notified =)) 

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/e55b8db2-9dca-42f9-8c48-3088c7bc7859)

`VirTool:Win32/PoshC2.G` 

Let's try checking its `sha256` to see how it goes, remember to turn off `windows security`
> Check to see if there's anything to lose, right?Check to see if there's anything to lose, right? hahaa

```
certutil -hashfile scanner.exe sha256

```

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/02a427a5-9694-477d-abd6-e1bffe0ec701)

### Q6) What scripting language is this C2 Framework primarily written in? (Format: Language)(1 points)



### Q7) What was the sending email address in question? (Format: mailbox@domain.tld)(1 points)

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/98d41a52-ea5b-463a-9da4-7a9afaee8d26)

`secretsociety2023@protonmail.com`

### Q8) What is the email subject name? (Format: Subject)(1 points)

`TurnOffAV&Run`

### Q9) Utilize CyberChef. After uploading the Base64 contents of the email into CyberChef, what is the defanged output of the href? (Format: hxxps[://]domain[.]tld/)(1 points)

Open it with `Notepad`

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/fce28968-ba9a-44be-9db5-448ddc2f8389)
















