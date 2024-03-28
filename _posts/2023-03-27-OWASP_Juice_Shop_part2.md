---
layout: post
title: OWASP Juice Shop Part 2
date: 2024-03-27 9:00 +0700
categories: [TryHackMe, Complete Beginner]
tags: [Blue Team, Complete Beginner]     # TAG names should always be lowercase
img_path: '/assets/THM'
image: 
  path: logo.png
--- 

## AH! Don't look!

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/33a599f8-4e0d-4652-9ccb-3e70893094f4)

A web application should store and transmit sensitive data safely and securely. But in some cases, the developer may not correctly protect their sensitive data, making it vulnerable.

Most of the time, data protection is not applied consistently across the web application making certain pages accessible to the public. Other times information is leaked to the public without the knowledge of the developer, making the web application vulnerable to an attack. 

More information: [Sensitive Data Exposure](https://owasp.org/www-project-top-ten/2017/A3_2017-Sensitive_Data_Exposure.html)

### Question #1: Access the Confidential Document!

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/7c989718-d480-4675-b9c9-deab0c14ff2a)

Navigate to the **About Us** page, and hover over the "Check out our terms of use".

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/b85bc3c4-b828-4caa-82ef-90a6ba33ae42)

You will see that it links to  `**http://MACHINE_IP/ftp/legal.md.**` Navigating to that /ftp/ directory reveals that it is exposed to the public!

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/cc8aec9c-5699-45e1-8ef4-354794ad6118)

We will download the **acquisitions.md** and save it. It looks like there are other files of interest here as well.

After downloading it, navigate to the **home page** to receive the flag!

`edf9281222395a1c5fee9b89e32175f1ccf50c5b`

### Question #2: Log into MC SafeSearch's account!

After watching the video there are certain parts of the song that stand out.

He notes that his password is "**Mr. Noodles**" but he has replaced some "**vowels into zeros**", meaning that he just replaced the o's into 0's.

We now know the password to the mc.safesearch@juice-sh.op account is "**Mr. N00dles**"

`66bdcffad9e698fd534003fbb3cc7e2b7b55d7f0`

### Question #3: Download the Backup file!

We will now go back to the  `**http://MACHINE_IP/ftp/**` folder and try to download **package.json.bak**. But it seems we are met with a 403 which says that only .md and .pdf files can be downloaded. 

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/1bd2a51a-9bd5-47b5-8263-aefbd88a631e)

To get around this, we will use a character bypass called "**Poison Null Byte**". A Poison Null Byte looks like this: **%00**. 

Note: as we can download it using the url, we will need to encode this into a url encoded format.

The Poison Null Byte will now look like this: **%2500**. Adding this and then a **.md** to the end will bypass the 403 error!

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/fee30b3e-fb40-4e01-9dc7-30312b803986)

**Why does this work?** 

A Poison Null Byte is actually a **NULL terminator**. By placing a NULL character in the string at a certain byte, the string will tell the server to terminate at that point, nulling the rest of the string. 

`bfc1e6b4a16579e85e06fee4c36ff8c02fb13795`

## Who's flying this thing?

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/bbe37885-6ec8-41fe-8b6d-74a1359b37f4)

Modern-day systems will allow for multiple users to have access to different pages. Administrators most commonly use an administration page to edit, add and remove different elements of a website. You might use these when you are building a website with programs such as Weebly or Wix.  

When Broken Access Control exploits or bugs are found, it will be categorised into one of **two types**:

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/3307d46f-4ea8-4789-8574-c502d0fd9ddf)

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/4cccf91d-507d-4712-bfb0-53ff67db8333)

More information: [Broken Access Control](https://owasp.org/www-project-top-ten/2017/A5_2017-Broken_Access_Control.html)

### Question #1: Access the administration page!

First, we are going to open the **Debugger** on **Firefox**. 

(Or **Sources** on **Chrome**.)

This can be done by navigating to it in the Web Developers menu. 

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/6500f9ad-dd85-43b6-9370-3d973f58f896)![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/7c77c1cd-1fef-48a4-9a5f-ad191e895af5)

We are then going to refresh the page and look for a javascript file for **main-es2015.js**

We will then go to that page at: `http://MACHINE_IP/main-es2015.js`

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/678b3895-6215-4067-b4fe-dd10223b7a47)

To get this into a format we can read, click the { } button at the bottom ![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/c10cc374-9327-4128-9c75-b176dabf775b)

Now search for the term "admin" 

You will come across a couple of different words containing "admin" but the one we are looking for is "path: administration"

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/ae6f2481-3e0e-4e98-a49e-4a90a656d349)

This hints towards a page called "**/#/administration**" as can be seen by the **about** path a couple lines below, but going there while not logged in doesn't work. 

As this is an Administrator page, it makes sense that we need to be in the **Admin account** in order to view it.

A good way to stop users from accessing this is to only load parts of the application that need to be used by them. This stops sensitive information such as an admin page from been leaked or viewed.

`946a799363226a24822008503f5d1324536629a0`

### Question #2: View another user's shopping basket!

Login to the Admin account and click on 'Your Basket'. Make sure Burp is running so you can capture the request!

Forward each request until you see: `GET /rest/basket/1 HTTP/1.1`

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/c9d6c609-4d4a-4965-8409-a83d856db97b)

Now, we are going to change the number **1** after /basket/ to **2**

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/a711c885-4c19-40a2-9cce-097bf59c0fb8)

It will now show you the basket of UserID 2. You can do this for other UserIDs as well, provided that they have one!

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/950b7892-5261-40fa-95f8-66536ef8a86c)

`41b997a36cc33fbe4f0ba018474e19ae5ce52121`

### Question #3: Remove all 5-star reviews!

Navigate to the  `http://MACHINE_IP/#/administration` page again and click the bin icon next to the review with 5 stars!

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/5b4b29c0-51b5-4b8a-a368-2fb8857366c1)

`50c97bcce0b895e446d61c83a21df371ac2266ef`

## Where did that come from?

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/5136f97d-6925-418d-b109-e8981c736605)

XSS or Cross-site scripting is a vulnerability that allows attackers to run javascript in web applications. These are one of the most found bugs in web applications. Their complexity ranges from easy to extremely hard, as each web application parses the queries in a different way. 

**There are three major types of XSS attacks:**

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/89dedcb1-ffea-4bad-a65d-43c55b7dbad2)

More information: [Cross-Site Scripting XSS](https://owasp.org/www-project-top-ten/2017/A7_2017-Cross-Site_Scripting_(XSS).html)

### Question #1: Perform a DOM XSS!

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/c5c7cde5-0f57-47b9-a6cf-8b5594dc0173)

We will be using the iframe element with a javascript alert tag: 

```
<iframe src="javascript:alert(`xss`)">

```

Inputting this into the **search bar** will trigger the alert.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/41cddd87-fc98-45d9-954a-d68ed2ac29cb)

Note that we are using **iframe** which is a common HTML element found in many web applications, there are others which also produce the same result. 

This type of XSS is also called XFS (Cross-Frame Scripting), is one of the most common forms of detecting XSS within web applications.

Websites that allow the user to modify the iframe or other DOM elements will most likely be vulnerable to XSS.   
**Why does this work?**

It is common practice that the search bar will send a request to the server in which it will then send back the related information, but this is where the flaw lies. Without correct input sanitation, we are able to perform an XSS attack against the search bar. 

`9aaf4bbea5c30d00a1f5bbcfce4db6d4b0efe0bf`

Question #2: Perform a persistent XSS!

First, login to the **admin** account.

We are going to navigate to the "**Last Login IP**" page for this attack.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/d0b18deb-eb33-4fda-b841-b0200f3c5689)

It should say the last IP Address is `0.0.0.0` or `10.x.x.x `

As it logs the 'last' login IP we will now logout so that it logs the 'new' IP.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/c9178329-1816-4922-8484-c0d2bddf80ec)

Make sure that Burp **intercept is on**, so it will catch the logout request.

We will then head over to the Headers tab where we will add a new header:

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/574c1c45-afa6-4324-8ff3-e5e2f08c6802)

Then forward the request to the server!

When **signing back into the admin account** and navigating to the Last Login IP page again, we will see the XSS alert!

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/43a6b6a8-cb90-4b7e-865c-f38ccf029b12)

**Why do we have to send this Header?**

The True-Client-IP  header is similar to the X-Forwarded-For header, both tell the server or proxy what the IP of the client is. Due to there being no sanitation in the header we are able to perform an XSS attack. 

`149aa8ce13d7a4a8a931472308e269c94dc5f156`

### Question #3: Perform a reflected XSS!

First, we are going to need to be on the right page to perform the reflected XSS!

**Login** into the **admin account** and navigate to the `**Order History**` page. 

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/9aff019d-a336-4f85-b5ab-64437832f6db)

From there you will see a "**Truck**" icon, clicking on that will bring you to the track result page. You will also see that there is an id paired with the order.   

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/571f12f1-7c73-41e1-89eb-6c5440193778)

We will use the iframe XSS, 
```
<iframe src="javascript:alert(`xss`)">

```
in the place of the 5267-f73dcd000abcc353

After submitting the URL, refresh the page and you will then get an alert saying XSS!

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/b92aade0-af97-4969-9759-789cd7eb08f7)

**Why does this work?**

The server will have a lookup table or database (depending on the type of server) for each tracking ID. As the 'id' parameter is not sanitised before it is sent to the server, we are able to perform an XSS attack.  

23cefee1527bde039295b2616eeb29e1edc660a0

## Exploration!

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/c2095a60-4c9b-4234-8667-ad9c2d03c631)

If you wish to tackle some of the **harder** challenges that were not covered within this room, check out the **/#/score-board/** section on Juice-shop. Here you can see your completed tasks as well as other tasks in varying difficulty.

### Access the /#/score-board/ page

`7efd3174f9dd5baa03a7882027f2824d2f72d86e`

