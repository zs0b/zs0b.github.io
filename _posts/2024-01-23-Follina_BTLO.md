---
layout: post
title: Follina 
date: 2024-01-23 16:00 +0700
categories: [BTLO, Incident Response]
tags: [VirusTotal, Any.Run, OSINT, Blue Team, btlo]     # TAG names should always be lowercase
img_path: '/assets/BTLO/Follina'
image: 
  path: logo.png
--- 

### Question 1) What is the SHA1 hash value of the sample? (Format: SHA1Hash) (1 points)

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/b65d6da6-ef77-4f56-9441-a3a1aa38659a)

### Question 2) According to VirusTotal, what is the full filetype of the provided sample? (Format: X X X X)

Upload files to **VirusTotal**, via detail tab ~~

### Question 3) Extract the URL that is used within the sample and submit it (Format: https://x.domain.tld/path/to/something) (1 points)

On **VirusTotal**, the tag is docx. Yes, we will change the file extension to zip and extract it

Docx file is a file consisting of compressed **XML**, inside it contains a file `document.xml.rels`, open it and you will see

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/e842eb06-d5de-4123-945c-ab6329b09682)

### Question 4) What is the name of the XML file that is storing the extracted URL? (Format: file.name.ext) (1 points)

You already know it lol 

### Question 5) The extracted URL accesses a HTML file that triggers the vulnerability to execute a malicious payload. According to the HTML processing functions, any files with fewer than <Number> bytes would not invoke the payload. Submit the <Number> (Format: Number of Bytes) (1 points)

`4096`

### Question 6) After execution, the sample will try to kill a process if it is already running. What is the name of this process? (Format: filename.ext) (1 points)
Since this website is no longer online, please refer to this article [HunTress](https://www.huntress.com/blog/microsoft-office-remote-code-execution-follina-msdt-bug) :_)

### Question 7) You were asked to write a process-based detection rule using Windows Event ID 4688. What would be the ProcessName and ParentProcessname used in this detection rule? [Hint: OSINT time!] (Format: ProcessName, ParentProcessName) (1 points)

It's pretty easy, you can search GG and you'll see it, if you haven't seen it yet, here it is ~

`msdt.exe, WINWORD.exe`

### Question 8) Submit the MITRE technique ID used by the sample for Execution [Hint: Online sandbox platforms can help!] (Format: TXXXX) (1 points)

As you know, this vulnerability is operated by CMD and please refer to [Mitre](https://attack.mitre.org/tactics/TA0002/) for an answer soon.

### Question 9) Submit the CVE associated with the vulnerability that is being exploited (Format: CVE-XXXX-XXXXX) (2 points)

Follina vulnerability is quite common, you just need to search Google to see the answer

goodbye, thank you for reading until now //~//
