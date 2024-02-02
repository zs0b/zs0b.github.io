---
layout: post
title: Source
date: 2024-02-02 7:00 +0700
categories: [BTLO, Reverse Engineering]
tags: [Manual Code Review, Blue Team, btlo, reverse ]     # TAG names should always be lowercase
img_path: '/assets/BTLO'
image: 
  path: logo.png
--- 

Ok, let's continue with the `Source` challenge

### What is the technology affected? (5 points)

I will open the file `zlib.c` with `VS code`

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/9f5aa516-07db-4597-a985-6f8407c55747)

Sure, I'm a chicken in this area so I had to search everything to find out .-.

![Alt text](https://media.giphy.com/media/BlEfRAkBfEkHS/giphy.gif?cid=ecf05e47xeuoc747mputk47ncv411dz2j8xrupqozr31hgyf&ep=v1_gifs_search&rid=giphy.gif&ct=g)

First, I found out what `SAPI.h` is used with `php.h` for 
> As you can see, `SAPI.h` is located entirely in the main function

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/1376fe0c-b4d4-4741-b507-c0dde884fcf3)

but... I don't know what it does @~@ so instead of searching on GG, I usually ask `GPT chat`
> Simply put, it contains definitions and functions related to the `Server API` that allow `PHP` to work with many different web servers

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/a746aa97-a371-449e-81c9-1335db5697d0)

After understanding, `ext/standard` for extension support etc

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/2d9e3a19-39c0-48fc-8ec6-ff704b91b98a)

I'm learn about `Zend/zend_interfaces.h`
> `Zend Engine` is a key component of `PHP runtime`, providing higher performance and features for PHP applications

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/92a2592c-f19e-4105-8363-e68dcc208318)

<i class="fa fa-long-arrow-right"></i> Grab the skirt, it's aiming for `php` =)))

### Based on the list of vulnerability categories in the challenge scenario, which one describes the identified vulnerability? (5 points)

I found [report](https://news.sophos.com/en-us/2021/03/30/php-web-language-narrowly-avoids-dangerous-supply-chain-attack/) about this vulnerability
>If you want to understand more about this vulnerability, and how it works, go to `report` to see 0.<

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/53bbdbbd-6a8d-41e3-ad1f-3cc5152bfec4)

Based on the information I read in the newspaper and the list of vulnerabilities that the challenge has listed
> Vulnerability Categories (Use this list to answer the related question. Example: Path Traversal): 1. Authentication Bypass 2. Buffer Overflow 3. Code Execution 4. Command Execution 5. Cryptographic flaw 6. Cross Origin Resource Sharing bypass 7. File Inclusion 8. Insecure Direct Object Reference 9. Insecure Deserialization 10. Path Traversal 11. Race Condition 12. Server-Side Request Forgery 13. Server-Side Template Injection 14. SQL Injection 15. XML External Entity

`Command Execution`

### See the corresponding commit. How many lines of code were added when the vulnerability was introduced? (5 points)

Based on a developer's main statement related to the event
> Read it [here](https://news-web.php.net/php.internals/113838)

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/4b4b5377-5d82-4b17-9ce4-26b6e51e7bd1)


There is a link to php zlib commit history on github, showing code changes

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/0b3e2a80-4e82-41f8-a852-3e66abe8163d)

`11`

### What HTTP head is required to exploit the vulnerability? (5 points)

`User-Agent`

![Alt text](https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExYzk4ZHJrMzJqZ3V5dnFhb3duYWJwZHZuZnUybXA4Mm5oamhzZ3o4aSZlcD12MV9naWZzX3NlYXJjaCZjdD1n/k0GtyU1I6a5NJ5hKRa/giphy.gif)

goodbye, thank you for reading until now //~//














