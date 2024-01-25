---
layout: post
title: Melissa 
date: 2024-01-25 13:00 +0700
categories: [BTLO, Reverse Engineering]
tags: [macro, virus, oledump, text editor, Blue Team, btlo, Reverse, Melissa]     # TAG names should always be lowercase
img_path: '/assets/BTLO/Melissa'
image: 
  path: logo.png
--- 

Hello again comrades, after lunch, let's jump in and continue with the `Melissa` challenge.

Melissa aka W97M.Melissa.A (Symantec) or Virus:W32/Melissa (F-Secure) is a macro virus dates back to March 26, 1999. As far as I know, it targets systems based on Microsoft Word and Outlook

This time it is transmitted via email with the subject line `"Important message from"` followed by the name of the current user, you will understand immediately lol, inside it says `" Here's that document you asked for. Don't show anyone else ;) "`, followed by `list.doc`, contains porn websites and login information for each site `( wow ;))) )`. and then it sends them to the FIRST 50 CONTACTS, it also disables many protection features on `Microsoft Word` and `Outlook` 

Enough !!!, come on

shjt, don't forget the warning

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/d727b621-57f7-476c-84c5-69633f76fd61)

### Submit the stream number that contains the Melissa macro in the LIST.DOC file (1 points)

First, please go here to download [oledump](https://blog.didierstevens.com/programs/oledump-py/) 

Then throw in this `Melissa`

```
python ./oledump.py LIST.DOC 

```

I see the answer

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/e9fbed84-cba2-45b7-b396-9d380b42fed5)

`7` 

### After identifying which version of word, Melissa will enable all macros from registry (1 points)

Now let's continue to investigate it further

```
python ./oledump.py -s 7 -v LIST.DOC

```

So it checked that Word 9.0 was running and...

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/180174a7-dd58-473c-acb7-24218ac9d90f)

`9.0`

### What is the email service targeted by Melissa (1 points)

You've seen it, not in the photo above

`Outlook`

### How many number of email addresses were collected (1 points)

It will repeat until 50 is reached

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/167d9ecc-d848-4e45-9560-88e48757e08d)

`50`

### What is the string used by melissa to identify whether a PC is infected or not and decide whether to collect email addresses or not (2 points)

It's easy, isn't it ? .--.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/4bf33de2-1327-454f-a0cf-c9014c1baee3)

`… by Kwyjibo`

### What is the variable responsible for identifying the email username of the infected PC (2 points)

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/6270027c-0731-423c-9529-5c7e10fcc015)

`Application.UserName`

### What is the text in email body used for spreading melissa (1 points)
 
In the photo above .-.

`Here is that document you asked for … don’t show anyone else ;-)`

### What is the text that is inserted by Melissa in an open word document? (1 points)

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/686436c9-a5d3-4751-823c-17ecf9ebfcab)

`Twenty-two points, plus triple-word-score, plus fifty points for using all my letters. Game’s over. I’m outta here.`

Well, this challenge is over, see you tomorrow

goodbye, thank you for reading until now //~//




















