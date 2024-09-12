---
layout: post
title: Network Services 2 part 3
date: 2024-03-18 9:00 +0700
categories: [TryHackMe, Complete Beginner]
tags: [Blue Team, Complete Beginner]     # TAG names should always be lowercase
img_path: '/assets/THM'
image: 
  path: logo.png
--- 

## Understanding MySQL

### What is MySQL?

In its simplest definition, MySQL is a relational database management system (RDBMS) based on Structured Query Language (SQL). Too many acronyms? Let's break it down:

### Database:

A database is simply a persistent, organised collection of structured data

### RDBMS:

A software or service used to create and manage databases based on a relational model. The word "relational" just means that the data stored in the dataset is organised as tables. Every table relates in some way to each other's "primary key" or other "key" factors.

### SQL:

MYSQL is just a brand name for one of the most popular RDBMS software implementations. As we know, it uses a client-server model. But how do the client and server communicate? They use a language, specifically the Structured Query Language (SQL).

Many other products, such as PostgreSQL and Microsoft SQL server, have the word SQL in them. This similarly signifies that this is a product utilising the Structured Query Language syntax.

### How does MySQL work?

MySQL, as an RDBMS, is made up of the server and utility programs that help in the administration of MySQL databases.

The server handles all database instructions like creating, editing, and accessing data. It takes and manages these requests and communicates using the MySQL protocol. This whole process can be broken down into these stages:

- MySQL creates a database for storing and manipulating data, defining the relationship of each table.

- Clients make requests by making specific statements in SQL.

- The server will respond to the client with whatever information has been requested.

### What runs MySQL?

MySQL can run on various platforms, whether it's Linux or windows. It is commonly used as a back end database for many prominent websites and forms an essential component of the LAMP stack, which includes: Linux, Apache, MySQL, and PHP.

### More Information:

Here are some resources that explain the technical implementation, and working of, MySQL in more detail than I have covered here:

[https://dev.mysql.com/doc/dev/mysql-server/latest/PAGE_SQL_EXECUTION.html](https://dev.mysql.com/doc/dev/mysql-server/latest/PAGE_SQL_EXECUTION.html)

[https://www.w3schools.com/php/php_mysql_intro.asp](https://www.w3schools.com/php/php_mysql_intro.asp)

### What type of software is MySQL?

`relational database management system`

### What language is MySQL based on?

`SQL`

### What communication model does MySQL use?

`client-server`

### What is a common application of MySQL?

`back end database`

### What major social network uses MySQL as their back-end database? This will require further research.

`Facebook`

## Enumerating MySQL

### Let's Get Started

Before we begin, make sure to deploy the room and give it some time to boot. Please be aware, as this can take up to five minutes, so be patient!

### When you would begin attacking MySQL

MySQL is likely not going to be the first point of call when getting initial information about the server. You can, as we have in previous tasks, attempt to brute-force default account passwords if you really don't have any other information; however, in most CTF scenarios, this is unlikely to be the avenue you're meant to pursue.

### The Scenario

Typically, you will have gained some initial credentials from enumerating other services that you can then use to enumerate and exploit the MySQL service. As this room focuses on exploiting and enumerating the network service, for the sake of the scenario, we're going to assume that you found the **credentials: "root:password"** while enumerating subdomains of a web server. After trying the login against SSH unsuccessfully, you decide to try it against MySQL.

### Requirements

You will want to have MySQL installed on your system to connect to the remote MySQL server. In case this isn't already installed, you can install it using `sudo apt install default-mysql-client`. Don't worry- this won't install the server package on your system- just the client.

Again, we're going to be using Metasploit for this; it's important that you have Metasploit installed, as it is by default on both Kali Linux and Parrot OS.

### Alternatives

As with the previous task, it's worth noting that everything we will be doing using Metasploit can also be done either manually or with a set of non-Metasploit tools such as nmap's mysql-enum script: [https://nmap.org/nsedoc/scripts/mysql-enum.html](https://nmap.org/nsedoc/scripts/mysql-enum.html) or [https://www.exploit-db.com/exploits/23081](https://www.exploit-db.com/exploits/23081). I recommend that after you complete this room, you go back and attempt it manually to make sure you understand the process that is being used to display the information you acquire.

Okay, enough talk. Let's get going!

### As always, let's start out with a port scan, so we know what port the service we're trying to attack is running on. What port is MySQL using?

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/469f92ce-c64f-4a7e-9701-4d4ae7f7257f)

`3306`

We're going to be using the "mysql_sql" module.

### Search for, select and list the options it needs. What three options do we need to set? (in descending order).

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/ff044f86-f888-43ca-bdaa-80a666a25d1b)

`PASSWORD/RHOSTS/USERNAME`

### Run the exploit. By default it will test with the "select version()" command, what result does this give you?

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/ec94c041-dcd9-4429-824f-7e4e7ee11f22)

`5.7.29-0ubuntu0.18.04.1`

### Great! We know that our exploit is landing as planned. Let's try to gain some more ambitious information. Change the "sql" option to "show databases". how many databases are returned?

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/518e86b1-ba0b-49a2-bdf0-8c69d1510a83)

`4`

## Exploiting MySQL

### What do we know?

Let's take a sanity check before moving on to try and exploit the database fully, and gain more sensitive information than just database names. We know:

1. MySQL server credentials

2. The version of MySQL running

3. The number of Databases, and their names.

### Key Terminology

In order to understand the exploits we're going to use next- we need to understand a few key terms.

### Schema:

In MySQL, physically, a schema is synonymous with a database. You can substitute the keyword "SCHEMA" instead of DATABASE in MySQL SQL syntax, for example using CREATE SCHEMA instead of CREATE DATABASE. It's important to understand this relationship because some other database products draw a distinction. For example, in the Oracle Database product, a schema represents only a part of a database: the tables and other objects owned by a single user.

### Hashes:

Hashes are, very simply, the product of a cryptographic algorithm to turn a variable length input into a fixed length output.

In MySQL hashes can be used in different ways, for instance to index data into a hash table. Each hash has a unique ID that serves as a pointer to the original data. This creates an index that is significantly smaller than the original data, allowing the values to be searched and accessed more efficiently

However, the data we're going to be extracting are password hashes which are simply a way of storing passwords not in plaintext format.

Lets get cracking.

### First, let's search for and select the "mysql_schemadump" module. What's the module's full name?

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/402f8b98-a01c-4ec4-8232-8cee70030862)

`auxiliary/scanner/mysql/mysql_schemadump`

### Great! Now, you've done this a few times by now so I'll let you take it from here. Set the relevant options, run the exploit. What's the name of the last table that gets dumped?

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/52f9a7c1-44e4-4ee3-8d4a-7d273caf3a7e)

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/e9b3bc98-4cf4-4b18-9bb0-6bb6126a330f)

`x$waits_global_by_latency`

### Awesome, you have now dumped the tables, and column names of the whole database. But we can do one better... search for and select the "mysql_hashdump" module. What's the module's full name?

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/1cdf000a-f35d-499d-a10e-c5a31e8d8bf8)

`auxiliary/scanner/mysql/mysql_hashdump`

### Again, I'll let you take it from here. Set the relevant options, run the exploit. What non-default user stands out to you?

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/d0475ff0-e869-4343-a76e-c3c13268d1ed)

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/6d5cb731-736e-4501-80f3-233ea0de3a95)

`carl`

Another user! And we have their password hash. This could be very interesting. Copy the hash string in full, like: bob:*HASH to a text file on your local machine called "hash.txt".

### What is the user/hash combination string?

`carl:*EA031893AA21444B170FC2162A56978B8CEECE18`

### Now, we need to crack the password! Let's try John the Ripper against it using: "john hash.txt" what is the password of the user we found?

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/2fed3024-373a-4207-bec1-c176797b3236)

`doggie`

Awesome. Password reuse is not only extremely dangerous, but extremely common. What are the chances that this user has reused their password for a different service?

### What's the contents of MySQL.txt

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/002b2842-20f7-4455-af48-5c611031a22c)

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/d02ba8df-0daa-4f23-a134-97378c091a55)

`THM{congratulations_you_got_the_mySQL_flag}`









































