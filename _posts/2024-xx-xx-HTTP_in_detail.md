---
layout: post
title: How The Web Works Part 2
date: 2024-02-16 8:00 +0700
categories: [TryHackMe, Pre Security]
tags: [Network Fundamentals, Blue Team]     # TAG names should always be lowercase
img_path: '/assets/THM'
image: 
  path: logo.png
--- 

## What is HTTP(S)?

### What is HTTP? (HyperText Transfer Protocol)

HTTP is what's used whenever you view a website, developed by Tim Berners-Lee and his team between 1989-1991. HTTP is the set of rules used for communicating with web servers for the transmitting of webpage data, whether that is HTML, Images, Videos, etc.

### What is HTTPS? (HyperText Transfer Protocol Secure)

HTTPS is the secure version of HTTP. HTTPS data is encrypted so it not only stops people from seeing the data you are receiving and sending, but it also gives you assurances that you're talking to the correct web server and not something impersonating it.

### What does HTTP stand for?

`HyperText Transfer Protocol`

### What does the S in HTTPS stand for?

`secure`

### On the mock webpage on the right there is an issue, once you've found it, click on it. What is the challenge flag?

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/bbbaf434-6d3a-463b-b202-d48a78cea9bc)

yeh :)

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/3e64ac0a-2e76-4a0c-972f-a55d0e95e53e)

`THM{INVALID_HTTP_CERT}`

## Requests And Responses

When we access a website, your browser will need to make requests to a web server for assets such as HTML, Images, and download the responses. Before that, you need to tell the browser specifically how and where to access these resources, this is where URLs will help.

### What is a URL? (Uniform Resource Locator)

If you’ve used the internet, you’ve used a URL before. A URL is predominantly an instruction on how to access a resource on the internet. The below image shows what a URL looks like with all of its features (it does not use all features in every request).

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/757b0c3e-528a-45cd-9e9a-ac769500e914)

`Scheme`: This instructs on what protocol to use for accessing the resource such as HTTP, HTTPS, FTP (File Transfer Protocol).

`User`: Some services require authentication to log in, you can put a username and password into the URL to log in.

`Host`: The domain name or IP address of the server you wish to access.

`Port`: The Port that you are going to connect to, usually 80 for HTTP and 443 for HTTPS, but this can be hosted on any port between 1 - 65535.

`Path`: The file name or location of the resource you are trying to access.

`Query String`: Extra bits of information that can be sent to the requested path. For example, /blog?id=1 would tell the blog path that you wish to receive the blog article with the id of 1.

`Fragment`: This is a reference to a location on the actual page requested. This is commonly used for pages with long content and can have a certain part of the page directly linked to it, so it is viewable to the user as soon as they access the page.

### Making a Request

It's possible to make a request to a web server with just one line `"GET / HTTP/1.1"`

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/5e29c0da-2ff0-4de9-9593-9afcc7fda17c)

But for a much richer web experience, you’ll need to send other data as well. This other data is sent in what is called headers, where headers contain extra information to give to the web server you’re communicating with, but we’ll go more into this in the Header task.

#### Example Request:

```
GET / HTTP/1.1
Host: tryhackme.com
User-Agent: Mozilla/5.0 Firefox/87.0
Referer: https://tryhackme.com/
```

To breakdown each line of this request:

`Line 1`: This request is sending the GET method ( more on this in the HTTP Methods task ), request the home page with / and telling the web server we are using HTTP protocol version 1.1.

`Line 2`: We tell the web server we want the website tryhackme.com

`Line 3`: We tell the web server we are using the Firefox version 87 Browser

`Line 4`: We are telling the web server that the web page that referred us to this one is [https://tryhackme.com](https://tryhackme.com)

`Line 5`: HTTP requests always end with a blank line to inform the web server that the request has finished.

#### Example Response:

```

HTTP/1.1 200 OK
Server: nginx/1.15.8
Date: Fri, 09 Apr 2021 13:34:03 GMT
Content-Type: text/html
Content-Length: 98

<html>
<head>
    <title>TryHackMe</title>
</head>
<body>
    Welcome To TryHackMe.com
</body>
</html>
```

To breakdown each line of the response:

`Line 1`: HTTP 1.1 is the version of the HTTP protocol the server is using and then followed by the HTTP Status Code in this case "200 Ok" which tells us the request has completed successfully.

`Line 2`: This tells us the web server software and version number.

`Line 3`: The current date, time and timezone of the web server.

`Line 4`: The Content-Type header tells the client what sort of information is going to be sent, such as HTML, images, videos, pdf, XML.

`Line 5`: Content-Length tells the client how long the response is, this way we can confirm no data is missing.

`Line 6`: HTTP response contains a blank line to confirm the end of the HTTP response.

`Lines 7-14`: The information that has been requested, in this instance the homepage.

### What HTTP protocol is being used in the above example?

`HTTP/1.1`

### What response header tells the browser how much data to expect?

`Content-Length`

## HTTP Methods

HTTP methods are a way for the client to show their intended action when making an HTTP request. There are a lot of HTTP methods but we'll cover the most common ones, although mostly you'll deal with the GET and POST method.

#### GET Request

This is used for getting information from a web server.

#### POST Request

This is used for submitting data to the web server and potentially creating new records

#### PUT Request

This is used for submitting data to a web server to update information

#### DELETE Request

This is used for deleting information/records from a web server.

### What method would be used to create a new user account?

`POST`

### What method would be used to update your email address?

`PUT`

### What method would be used to remove a picture you've uploaded to your account?

`DELETE`

### What method would be used to view a news article?

`GET`

## HTTP Status Codes

### HTTP Status Codes:

In the previous task, you learnt that when a HTTP server responds, the first line always contains a status code informing the client of the outcome of their request and also potentially how to handle it. These status codes can be broken down into 5 different ranges:

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/00afe970-7ddc-48b4-a7a6-ef793d587fb5)

### Common HTTP Status Codes:

There are a lot of different HTTP status codes and that's not including the fact that applications can even define their own, we'll go over the most common HTTP responses you are likely to come across:

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/bed5b941-f4be-4842-b783-2a0b4a46ec26)

Click the "View Site" button on the right to see what some of these HTTP status messages look like in a browser.

`403 Error`

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/5c0ff820-7184-4036-9af5-6ba0cf90ea9f)

`404 Error`

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/0ad3dfdf-0347-4fd7-ad10-7bf5917d116f)

`503 Error`

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/b197ac7e-1149-411d-a89b-35507e9553ed)

### What response code might you receive if you've created a new user or blog post article?

`201`

### What response code might you receive if you've tried to access a page that doesn't exist?

`404`

### What response code might you receive if the web server cannot access its database and the application crashes?

`503`

### What response code might you receive if you try to edit your profile without logging in first?

`401`

## Headers

Headers are additional bits of data you can send to the web server when making requests.

Although no headers are strictly required when making a HTTP request, you’ll find it difficult to view a website properly.

### Common Request Headers

﻿These are headers that are sent from the client (usually your browser) to the server.

`Host`: Some web servers host multiple websites so by providing the host headers you can tell it which one you require, otherwise you'll just receive the default website for the server.

`User-Agent`: This is your browser software and version number, telling the web server your browser software helps it format the website properly for your browser and also some elements of HTML, JavaScript and CSS are only available in certain browsers.

`Content-Length`: When sending data to a web server such as in a form, the content length tells the web server how much data to expect in the web request. This way the server can ensure it isn't missing any data.

`Accept-Encoding`: Tells the web server what types of compression methods the browser supports so the data can be made smaller for transmitting over the internet.


`Cookie`: Data sent to the server to help remember your information (see cookies task for more information).

### Common Response Headers

These are the headers that are returned to the client from the server after a request.

`Set-Cookie`: Information to store which gets sent back to the web server on each request (see cookies task for more information).

`Cache-Control`: How long to store the content of the response in the browser's cache before it requests it again.

`Content-Type`: This tells the client what type of data is being returned, i.e., HTML, CSS, JavaScript, Images, PDF, Video, etc. Using the content-type header the browser then knows how to process the data.

`Content-Encoding`: What method has been used to compress the data to make it smaller when sending it over the internet.

### What header tells the web server what browser is being used?

`User-Agent`

### What header tells the browser what type of data is being returned?

`Content-Type`

### What header tells the web server which website is being requested?

`Host`

## Cookies

You've probably heard of cookies before, they're just a small piece of data that is stored on your computer. Cookies are saved when you receive a "Set-Cookie" header from a web server. Then every further request you make, you'll send the cookie data back to the web server. Because HTTP is stateless (doesn't keep track of your previous requests), cookies can be used to remind the web server who you are, some personal settings for the website or whether you've been to the website before. Let's take a look at this as an example HTTP request:

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/fd98da23-dac0-46a3-9af8-af282a158e6a)

Cookies can be used for many purposes but are most commonly used for website authentication. The cookie value won't usually be a clear-text string where you can see the password, but a token (unique secret code that isn't easily humanly guessable).

### Which header is used to save cookies to your computer?

`Set-Cookie`

## Making Requests

This is an emulator for making demo HTTP requests, using what you've learnt from the above tasks you can use it to complete the below questions.

### Make a GET request to /room

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/d15c4333-b9ed-48dc-84df-c49e3b721472)

`THM{YOU'RE_IN_THE_ROOM}`

###  Make a GET request to /blog and using the gear icon set the id parameter to 1 in the URL field

Go to the gear icon

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/80c0b09f-3d99-4e33-b33e-842f7c8131d7)

After setting `id=1`, save it

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/286c885a-0c24-4363-9e96-0216f14f6650)

yeh =)) 

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/da132484-64bd-4fe9-81c0-6384abca7d00)

`THM{YOU_FOUND_THE_BLOG}`

### Make a DELETE request to /user/1

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/0b618d39-ff68-43b1-b45a-4460668c8251)

`THM{USER_IS_DELETED}`

### Make a PUT request to /user/2 with the username parameter set to admin

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/3864c751-293b-4c16-af41-5c6ed0369ece)

In the gear add the parameter `username=admin`

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/3eea040a-a24a-460b-bac2-e058073be39d)

right ~

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/581e6dc4-c11b-4790-a183-2de574a616a1)

`THM{USER_HAS_UPDATED}`

### POST the username of thm and a password of letmein to /login

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/e423c73c-dd38-474a-9bb6-80033ce62500)

Set parameters `username=thm` and `password=letmein`

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/8d78ec41-9884-4d1a-9303-535b97bfa980)

hereeeeeeeee

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/3f1701c2-b0fa-4024-b534-05b94cea1d25)

`THM{HTTP_REQUEST_MASTER}`

goodbye, thank you for reading until now //~//










