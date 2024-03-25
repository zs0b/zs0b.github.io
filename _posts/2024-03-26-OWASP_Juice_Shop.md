---
layout: post
title: OWASP Juice Shop
date: 2024-03-26 1:00 +0700
categories: [TryHackMe, Complete Beginner]
tags: [Blue Team, Complete Beginner]     # TAG names should always be lowercase
img_path: '/assets/THM'
image: 
  path: logo.png
--- 

## Let's go on an adventure!

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/b515f455-2eca-4197-94d7-e3b4c7f929c5)

Before we get into the actual hacking part, it's good to have a look around. In Burp, set the Intercept mode to off and then browse around the site. This allows Burp to log different requests from the server that may be helpful later. 

This is called walking through the application, which is also a form of reconnaissance!

### Question #1: What's the Administrator's email address?

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/b5fa481b-1c8d-45ad-90d9-a8ec88f77593)

The reviews show each user's email address. Which, by clicking on the Apple Juice product, shows us the Admin email!

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/1aff3566-49e9-4f8c-9c20-0c5b62a85cb3)

`admin@juice-sh.op`

### Question #2: What parameter is used for searching? 

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/324ec7e6-70f6-476c-b475-1f79b19b4970)

Click on the magnifying glass in the top right of the application will pop out a search bar.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/7960d298-d08f-4f8a-997d-213b73d37f86)

We can then input some text and by pressing Enter will search for the text which was just inputted.

Now pay attention to the URL which will now update with the text we just entered.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/7eae7218-04c3-45f6-8005-d081482709f5)

We can now see the search parameter after the /#/search? the letter q

`q`

### Question #3: What show does Jim reference in his review? 

Jim did a review on the Green Smoothie product. We can see that he mentions a replicator. 

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/9d426243-c882-4861-802b-b9ff59f6085c)

If we google "replicator" we will get the results indicating that it is from a TV show called Star Trek

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/0de5e4e0-5591-444a-b692-002ec1b1b94d)

`Star Trek`

##  Inject the juice

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/9052f0e0-ac64-4291-9183-bbab19cef0e2)

This task will be focusing on injection vulnerabilities. Injection vulnerabilities are quite dangerous to a company as they can potentially cause downtime and/or loss of data. Identifying injection points within a web application is usually quite simple, as most of them will return an error. There are many types of injection attacks, some of them are:

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/7075dd37-8511-4fb7-a998-e73565effa2b)

But in our case, we will be using **SQL Injection**.

For more information: [Injection](https://owasp.org/www-project-top-ten/2017/A1_2017-Injection.html)

### Question #1: Log into the administrator account!

After we navigate to the login page, enter some data into the email and password fields.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/3046537b-15c2-4b65-b6c2-4e4c3ba9491f)

**Before** clicking submit, make sure **Intercept mode is on**.

This will allow us to see the data been sent to the server!

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/37ad13b1-d115-4e75-96a8-e35381b28af2)

We will now change the "**a**" next to the email to: **' or 1=1--** and forward it to the server.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/a0f23ec7-413a-4159-8b61-c538a5b8d592)

**Why does this work?**

- The character ' will close the brackets in the SQL query

- '**OR**' in a SQL statement will return true if either side of it is true. As **1=1 is always true**, the whole statement is true. Thus it will tell the server that the email is valid, and log us into **user id 0**, which happens to be the administrator account.

- The -- character is used in SQL to comment out data, any restrictions on the login will no longer work as they are interpreted as a comment. This is like the **#** and **//** comment in python and javascript respectively.

`32a5e0f21372bcc1000a6088b93b458e41f0e02a`

### Question #2: Log into the Bender account!

Similar to what we did in **Question #1**, we will now log into Bender's account! Capture the login request again, but this time we will put: **bender@juice-sh.op'--** as the email. 

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/015e5fcf-0253-43ad-adf2-7bf1080cd67c)

Now, forward that to the server!

But why don't we put the **1=1**?

Well, as the email address is valid (which will return **true**), we do not need to force it to be **true**. Thus we are able to use '-- to bypass the login system. Note the **1=1** can be used when the email or username is not known or invalid.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/346a6e92-9c94-4bac-8298-2d235c4b4089)

`fb364762a3c102b2db932069c0e6b78e738d4066`

## Who broke my lock?!

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/4678bc47-ce17-4e64-a183-4fa33b06486e)

In this task, we will look at exploiting authentication through different flaws. When talking about flaws within authentication, we include mechanisms that are vulnerable to manipulation. These mechanisms, listed below, are what we will be exploiting. 

Weak passwords in high privileged accounts

Forgotten password pages

More information: Broken Authentication

### Question #1: Bruteforce the Administrator account's password!

We have used SQL Injection to log into the Administrator account but we still don't know the password. Let's try a brute-force attack! We will once again capture a login request, but instead of sending it through the proxy, we will send it to Intruder.

Go to Positions and then select the **Clear §** button. In the password field place two § inside the quotes. To clarify, the § § is not two sperate inputs but rather Burp's implementation of quotations e.g. "". The request should look like the image below. 

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/2b41a574-c73e-4f1e-a02e-132c1e81ed07)

For the payload, we will be using the **`best1050.txt` from Seclists**. (Which can be installed via: **apt-get install seclists**)

*You can load the list from: /usr/share/wordlists/SecLists/Passwords/Common-Credentials/best1050.txt*

Once the file is loaded into Burp, start the attack. You will want to filter for the request by status.

A **failed** request will receive a **401 Unauthorized** ![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/4a80c837-d787-41fe-aa4f-3996d4f6422f)

Whereas a **successful** request will return a **200 OK** ![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/cb2372ad-10ab-4264-bc57-7aff24ecdc0e)

Once completed, login to the account with the password.

`c2110d06dc6f81c67cd8099ff0ba601241f1ac0e`

### Question #2: Reset Jim's password!

Believe it or not, the reset password mechanism can also be exploited! When inputted into the email field in the Forgot Password page, Jim's security question is set to "*Your eldest siblings middle name?*".

In Task 2, we found that Jim might have something to do with **Star Trek**. Googling "Jim Star Trek" gives us a wiki page for **Jame T. Kirk** from Star Trek. 

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/7988813e-e710-4a53-a599-78a578739479)

Looking through the wiki page we find that he has a brother.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/62a273c4-5d55-4977-b792-ceee4d4564d2)

Looks like his brother's middle name is **Samuel**

Inputting that into the Forgot Password page allows you to successfully change his password.

You can change it to anything you want!

`094fbc9b48e525150ba97d05b942bbf114987257`



























































