---
layout: post
title: How websites work 
date: 2024-03-19 9:00 +0700
categories: [TryHackMe, Complete Beginner]
tags: [Blue Team, Complete Beginner]     # TAG names should always be lowercase
img_path: '/assets/THM'
image: 
  path: logo.png
--- 

## How websites work

By the end of this room, you'll know how websites are created and will be introduced to some basic security issues.

When you visit a website, your browser (like Safari or Google Chrome) makes a request to a web server asking for information about the page you're visiting. It will respond with data that your browser uses to show you the page; a web server is just a dedicated computer somewhere else in the world that handles your requests.

There are two major components that make up a website:

- Front End (Client-Side) - the way your browser renders a website.

- Back End (Server-Side) - a server that processes your request and returns a response.

There are many other processes involved in your browser making a request to a web server, but for now, you just need to understand that you make a request to a server, and it responds with data your browser uses to render information to you.

### What term best describes the component of a web application rendered by your browser?

`Front End`

## HTML

Websites are primarily created using:

- HTML, to build websites and define their structure

- CSS, to make websites look pretty by adding styling options

- JavaScript, implement complex features on pages using interactivity

HyperText Markup Language (HTML) is the language websites are written in. Elements (also known as tags) are the building blocks of HTML pages and tells the browser how to display content. The code snippet below shows a simple HTML document, the structure of which is the same for every website:

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/4275f245-181a-457d-aeec-a3db742c84f8)

The HTML structure (as shown in the screenshot) has the following components:

- The <!DOCTYPE html> defines that the page is a HTML5 document. This helps with standardisation across different browsers and tells the browser to use HTML5 to interpret the page.

- The <html> element is the root element of the HTML page - all other elements come after this element.

- The <head> element contains information about the page (such as the page title)

- The <body> element defines the HTML document's body; only content inside of the body is shown in the browser.

- The <h1> element defines a large heading

- The <p> element defines a paragraph
There are many other elements (tags) used for different purposes. For example, there are tags for buttons (<button>), images (<img>), lists, and much more. 


















































































































