--- 
layout: post
title: Malware Analysis - Ransomware Script
date: 2024-01-25 18:00 +0700
categories: [BTLO, Reverse Engineering]
tags: [Ransomware, Text Editor, Malware Analysis, Blue Team, btlo, Reverse]     # TAG names should always be lowercase
img_path: '/assets/BTLO/Malware_Analysis_Ransomware_Script'
image: 
  path: logo.png
--- 

You can understand `Ransomware` is a type of cryptovirological malware that permanently block access to the victim's personal data unless a ransom is paid.

Ok, now let's get into the challenge

Don't forget the warning part .-.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/5b490e45-73f3-4a5a-b544-ce7382165b99)

### What is the malicious IP address referenced multiple times in the script? (1 points)

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/70f3b012-da49-4984-8569-7dba7e7fcfe0)

`185.141.25.168`

### The script uses apt-get to retrieve two tools, and uses yum to install them. What is the command line to remove the yum logs afterwards? (1 points)

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/e7e4ed7b-05ed-4309-8841-f61fe7c3dee8)

`rm -rf /var/log/yum*`

### A message is created in the file /etc/motd. What are the three first words? (1 points)

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/b9bcccf3-c8a4-44e2-83f7-74e04788d8c2)

`You were hacked`

### This message also contains a contact email address to have the system fixed. What is it? (1 points)

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/4dd8ad92-d64c-4016-afc2-f700c5ee4961)

nationalsiense@protonmail.com

### When files are encrypted, an unusual file extension is used. What is it? (2 points)

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/bc43a154-6b67-49d5-9513-44b5f98355b7)

`☢` 

### There are 5 functions associated with the encryption process that start with ‘encrypt’. What are they, in the order they’re actually executed in the script? (do not include "()") (2 points)

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/7a5d58b5-f248-4f5e-bcf5-de64680909e0)

`encrypt_ssh, encrypt_grep_files, encrypt_home, encrypt_root, encrypt_db`

### The script will check a text file hosted on the C2 server. What is the full URL of this file? (2 points)

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/0e73bc59-3e0f-4a5f-b383-c99823e074aa)

http://185.141.25.168/check_attack/0.txt

goodbye, thank you for reading until now //~//





