---
layout: post
title: Introduction to Cyber Security part 1 
date: 2024-02-05 6:00 +0700
categories: [TryHackMe, Pre Security]
tags: [Pre Security, Blue Team]     # TAG names should always be lowercase
img_path: '/assets/THM'
image: 
  path: logo.png
--- 

# Intro to Offensive Security

## What is Offensive Security?

In short, offensive security is the process of breaking into computer systems, exploiting software bugs, and finding loopholes in applications to gain unauthorized access to them.

To beat a hacker, you need to behave like a hacker, finding vulnerabilities and recommending patches before a cybercriminal does, as you'll do in this room!

On the flip side, there is also defensive security, which is the process of protecting an organization's network and computer systems by analyzing and securing any potential digital threats; learn more in the digital forensics room.

In a defensive cyber role, you could be investigating infected computers or devices to understand how it was hacked, tracking down cybercriminals, or monitoring infrastructure for malicious activity.

### `Which of the following options better represents the process where you simulate a hacker's actions to find vulnerabilities in a system?`

`Offensive Security`

## Hacking your first machine

Your first hack

Click the "Start Machine" button. Once loaded in Split View in your browser, you will have access to a machine you'll use to hack a fake bank application called FakeBank. If you don't see the machine appear, use the blue Show Split View button on the top-right of this page.

We will use a command-line application called "GoBuster" to brute-force FakeBank's website to find hidden directories and pages. GoBuster will take a list of potential page or directory names and tries accessing a website with each of them; if the page exists, it tells you.

Step 1) Open a terminal

A terminal, also known as the command-line, allows us to interact with a computer without using a graphical user interface. On the machine, open the terminal using the Terminal

Step 2) Find hidden website pages

Most companies will have an admin portal page, giving their staff access to basic admin controls for day-to-day operations. For a bank, an employee might need to transfer money to and from client accounts. Often these pages are not made private, allowing attackers to find hidden pages that show, or give access to, admin controls or sensitive data.

Type the following command into the terminal to find potentially hidden pages on FakeBank's website using GoBuster (a command-line security application).

```
gobuster -u http://fakebank.com -w wordlist.txt dir

```

The command will run and show you an output similar to this:

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/50143735-b86d-4cfd-999f-4eaf2cd72c65)

In the command above, -u is used to state the website we're scanning, -w takes a list of words to iterate through to find hidden pages.

You will see that GoBuster scans the website with each word in the list, finding pages that exist on the site. GoBuster will have told you the pages it found in the list of page/directory names (indicated by Status: 200)

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/a0db465f-6fa2-467f-aaa3-0f6184abcdf9)

Step 3) Hack the bank

You should have found a secret bank transfer page that allows you to transfer money between accounts at the bank (/bank-transfer). Type the hidden page into the FakeBank website on the machine.

This page allows an attacker to steal money from any bank account, which is a critical risk for the bank. As an ethical hacker, you would (with permission) find vulnerabilities in their application and report them to the bank to fix before a hacker exploits them.

Transfer $2000 from the bank account 2276, to your account (account number 8881).

### `Above your account balance, you should now see a message indicating the answer to this question. Can you find the answer you need?`

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/c45b253f-be4b-4b47-bbac-b18f407dc894)

and then after that...

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/a751316e-c9db-4eae-b117-28c05994b204)

`BANK-HACKED`

## Careers in cyber security

`How can I start learning?`

People often wonder how others become hackers (security consultants) or defenders (security analysts fighting cybercrime), and the answer is simple. Break it down, learn an area of cyber security you're interested in, and regularly practice using hands-on exercises. Build a habit of learning a little bit each day on TryHackMe, and you'll acquire the knowledge to get your first job in the industry.

Trust us; you can do it! Just take a look at some people who have used TryHackMe to get their first security job:

- Paul went from a construction worker to a security engineer. [Read more](https://tryhackme.com/r/resources/blog/construction-worker-to-security-engineer-how-paul-used-tryhackme-to-land-his-first-job-in-security).

- Kassandra went from a music teacher to a security professional. [Read more](https://tryhackme.com/r/resources/blog/the-teacher-becomes-the-student).

- Brandon used TryHackMe while at school to get his first job in cyber. [Read more](https://tryhackme.com/r/resources/blog/brandons-success-story).

`What careers are there?` 

The cyber careers room goes into more depth about the different careers in cyber. However, here is a short description of a few offensive security roles:

- Penetration Tester - Responsible for testing technology products for finding exploitable security vulnerabilities.

- Red Teamer - Plays the role of an adversary, attacking an organization and providing feedback from an enemy's perspective.

- Security Engineer - Design, monitor, and maintain security controls, networks, and systems to help prevent cyberattacks.

goodbye, thank you for reading until now //~//

![Alt text](https://media.giphy.com/media/qiMbLh4WHEZyw/giphy.gif?cid=790b7611iegnq76xglpshccvv18cbqxo935qhpyh9tiyl2g4&ep=v1_gifs_trending&rid=giphy.gif&ct=g) 
