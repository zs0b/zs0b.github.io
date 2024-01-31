--- 
layout: post
title: PowerShell Analysis - Keylogger
date: 2024-01-28 11:00 +0700
categories: [BTLO, Reverse Engineering]
tags: [Text Editor, Malware Analysis, Blue Team, btlo, Reverse, Keylogger]     # TAG names should always be lowercase
img_path: '/assets/BTLO/PowerShell_Analysis_Keylogger'
image: 
  path: logo.png
--- 

Hi everyone 

As always let's find out what it is

Keylogger is the action of recording (logging) the keys struck on a keyboard, typically covertly, so that a person using the keyboard is unaware that their actions are being monitored.

Don't ignore the warning

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/06317f67-d707-4f28-9601-eeada1c44b6a)

### What is the SHA256 hash value for the PowerShell script file? (1 points)

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/1265db34-ca1d-4a1c-985d-3788d34632e3)

`e0b7a2ad2320ac32c262aeb6fe2c6c0d75449c6e34d0d18a531157c827b9754e`

### What email address is used to send and receive emails? (1 points)

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/5d792741-c4f3-4ef0-9b99-7ab7513a7d93)

`chaudhariparth454@gmail.com`

### What is the password for this email account? (2 points)

2 questions are too easy .-.

`yjghfdafsd5464562!`

### What port is used for SMTP? (2 points)

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/4797a940-42f9-4489-bad3-306d4003ec25)

`587`

### What DLL is imported to help record keystrokes? (2 points)

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/5a002b9e-536f-4380-8243-1376daf45fb9)

`user32.dll`

### What directory is the generated txt file put in? (2 points)

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/4d57a1cf-9639-4f4f-bbbf-f0334d3691ab)

`temp\keylogger.txt` 

goodbye, thank you for reading until now //~//

