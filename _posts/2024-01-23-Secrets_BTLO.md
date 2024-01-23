---
layout: post
title: Secrets 
date: 2024-01-23 16:00 +0700
categories: [BTLO, Incident Response]
tags: [Python, Blue Team, btlo]     # TAG names should always be lowercase
img_path: '/assets/BTLO/Secrets'
image: 
  path: logo.png
--- 

### 1) Can you identify the name of the token? (Format: String) (2 points)

We will upload the ticket to [CyberChef](https://gchq.github.io/CyberChef/) to check

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/db69207d-6664-4bd4-9281-97a6553cf9f7)

Search GG and see [JWT decode](https://jwt.io/) 

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/0971ca40-e083-4255-8f4f-c1cf0833457a)

### 2) What is the structure of this token? (Format: Section.Section.Section) (2 points)

As seen, it includes: `Header, Payload, Signature` 

### 3) What is the hint you found from this token? (Format: String) (2 points)

As seen "flag": "BTL{_4_Eyes}". The verdict will be `_4_Eyes`

### 4) What is the Secret? (Format: String) (2 points)

To decode we will use brute force, there is hashcat and john here I will use hashcat

You must first go to `root` 

```
hashcat token.txt -m 16500 -a 3 ?a?a?a?a

```

- `-m 16500` in Hashcat is used to crack JSON Web Tokens (JWT)

  ![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/22f2fe83-83d4-4a24-bdc5-60802c7502b2)
- `-a 3 ?a?a?a?a` in Hashcat is used for brute force
  ![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/a0eff1cc-e239-4869-90c0-739c4f0d9c4a)

Since I already did it, I used `Hashcat --show`

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/78334a42-1bd8-49ec-8ca8-bff33e9043c5)

yeh so the secret here is `bT!0`

### 5) Can you generate a new verified signature ticket with a low privilege? (Format: String.String.String) (2 points)

To get the answer to this question, there is a secret in the previous question, now change the value of admin from `true` to `false`

Because I don't have admin rights hehe :>

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/b8523e03-4741-4bad-a07e-a6d13854f9fa)

`eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJmbGFnIjoiQlRMe180X0V5ZXN9IiwiaWF0Ijo5MDAwMDAwMCwibmFtZSI6IkdyZWF0RXhwIiwiYWRtaW4iOmZhbHNlfQ.nMXNFvttCvtDcpswOQA8u_LpURwv6ZrCJ-ftIXegtX4 `

goodbye, thank you for reading until now //~//





