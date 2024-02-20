---
layout: post
title: BRONCO CTF 
date: 2024-02-20 16:00 +0700
categories: [Writeup, BRONCO 2024]
tags: [forensics, cryto, web, OSINT, BRONCO CTF]     # TAG names should always be lowercase
img_path: '/assets/BroncoCTF'
image: 
  path: logo.png
--- 

Hi comrades :>>>

I can't explain much, so let's get started

# Keyboard Elitist

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/a118e81d-75ad-411a-8245-a9fedb3bc59b)

```
A;;apfkgij gj;ukd ar ut ghur war a Qwfpgj efjbyaps yk a Cyifmae uk;lg rchfmf maefr ghur iyye iuef dapbadf. Mj tpufks ur sftukugfij a efjbyaps rkyb, ylg hfpf wugh hur mysliap tpamfwype ia;gy;. Rudh, fughfp waj... hfpf ur ghf tiadO bpykcy{qwfpgj_vr_c0ifm@e}

```

I came here ([Dcode](https://www.dcode.fr/monoalphabetic-substitution)) to find the answer, it's quite easy 

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/28dea92e-ab98-44d7-b886-b6de1a8bca3c)

`BRONCO{QWERTY_VS_C0LEM@K}`

# Shrekanana Banana

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/07aa69af-e352-4c93-a7fe-7138affbb4f9)

This article I used [StegOline](https://georgeom.net/StegOnline/upload)

Move to `Green 2`

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/a3fcb6b1-bf90-47a1-bff1-6076347bd68c)

`bronco{shr3konogr@phy}` 

# Stego-Snore-Us

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/70d094b4-f660-444a-9279-aadd14c99e3d)

Use [StegOnline](https://georgeom.net/StegOnline/image) to navigate to `Blue 1`

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/94056bff-02d6-4277-a27c-7d53ff0c329b)

Push looks like cipher using [quipqiup](https://quipqiup.com/) to decode it

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/6b4f5faa-1bb8-473e-87cb-1fec058ac206)

`bronco{no_more_all_nighters}`

# ACM Borg Members

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/e5df16c8-ac7e-4584-addc-105407afcab3)

The question refers to `Santa Clara's ACM`, looked it up on GG and found this site
> `https://www.scuacm.com/`

If it mentions `cyborgs` try looking for it
> `https://www.scuacm.com/robots.txt`

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/a04016b9-41e7-407f-8d1f-48764631a4ff)

`bronco{be3p_b0op_@CM_are_cyb0rgs}`

# Blue Boy Storage

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/13cc56de-00c4-4a2f-8360-bfc8ae44a765)

Follow me, comrades

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/0e76857c-94f6-45a8-87c0-f069e5f802c1)

Here, I always look for the format of the first ctf, and boom
>haha, It was by chance

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/a1526913-910a-41b7-b8f5-539f6d14afe9)

`broncoctf{ab4_d3_4ba_d1e_1m_blu3}` 

# All I Do Is

Tried accessing the website `https://diamonds.broncoctf.xyz` but it didn't work @~@

I tried watching the YouTube video
> right, pay attention to the title `All I Do Is Dig`

>Maybe the question asks to use `Dig`, ok let's try it

References: 

```
https://www.cloudflare.com/learning/dns/dns-records/

```

```
zs0b@b3r0th3rcc  ~  dig diamonds.broncoctf.xyz  

; <<>> DiG 9.19.19-1-Debian <<>> diamonds.broncoctf.xyz
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 64662
;; flags: qr rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;diamonds.broncoctf.xyz.		IN	A

;; AUTHORITY SECTION:
broncoctf.xyz.		145	IN	SOA	ns-cloud-e1.googledomains.com. cloud-dns-hostmaster.google.com. 1 21600 3600 259200 300

;; Query time: 12 msec
;; SERVER: 123.26.26.26#53(123.26.26.26) (UDP)
;; WHEN: Mon Feb 19 14:58:51 +07 2024
;; MSG SIZE  rcvd: 144

 zs0b@b3r0th3rcc  ~  dig TXT diamonds.broncoctf.xyz 

; <<>> DiG 9.19.19-1-Debian <<>> TXT diamonds.broncoctf.xyz
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 24510
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;diamonds.broncoctf.xyz.		IN	TXT

;; ANSWER SECTION:
diamonds.broncoctf.xyz.	300	IN	TXT	"bronco{Finding_diamonds_aint_so_hard_just_dig_baby_dig}"

;; Query time: 56 msec
;; SERVER: 123.26.26.26#53(123.26.26.26) (UDP)
;; WHEN: Mon Feb 19 14:59:00 +07 2024
;; MSG SIZE  rcvd: 119
```

`bronco{Finding_diamonds_aint_so_hard_just_dig_baby_dig}`

# Blue Herring

I found it quite easy to try downloading it and checking it out

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/f5ed55d0-cce9-4147-bf04-da0873970e92)

Use `zsteg` and i found it .-.

```
zsteg --all BlueHerring-CPmowcv1.jpeg

```

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/d5c21e92-45e4-4ccc-bafc-68df517f1293)

`broncoctf{D1s_H3rr1ng_Sh0uld4_B33n_Blue}`

# Medieval Beats

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/c27d4fcb-7e2b-4fc1-afc0-877dd3ac2c71)

After checking, the flags are randomly hidden inside the video, download it, and use ffmpeg to process the images.

```
ffmpeg -i y2mate.com\ -\ Flag\ Vido_1080p.mp4 -vf "fps=1" output_%04d.png

```

The black ones have 6230 bytes of data, delete it

```
find . -type f -size 6230c -exec rm {} +

```

That's right, now there is a flag

`bronco{1n_17_f0r_7h3_10n6_h4ul}`

# Wario Party

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/ea6ce117-2da6-436e-857d-2e0d62ab3aa2)

Using [Aperi' Solve](https://www.aperisolve.com/) to check, I found that on the second bitplane something was encrypted

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/a2131097-2613-4697-a50c-8dc91552c944)

Use StegOnline to extract it

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/76c647cd-dd0c-4ac3-b167-06a747dc0ca1)

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/05b3b077-d7f1-4357-bea2-d9aebea7f181)

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/e250c0d5-471a-483c-bbee-2e77735c5a50)

I guess maybe it's binary .-. manually go into [CyberChef](https://gchq.github.io/CyberChef/) and see what it comes up with

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/35e044fa-45b4-4b14-abb3-0f7c297d2dc7)

yayyyyy ~ out there

```
01100010 01110010 01101111 01101110 01100011 01101111 01110011 01100101 01100011 01111011 01100010 00110000 01110111 01110011 00110011 01110010 01011111 01100111 00110000 01110100 01011111 01110100 01101000 00110100 01110100 01011111 01100100 01110101 01101101 01110000 01111001 01111101

```

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/f610cdd3-8fa2-4ef0-9365-2f94bcac57c2)

`broncosec{b0ws3r_g0t_th4t_dumpy}`

# Boom

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/b4853c9f-a909-4f6e-9c92-3077372a3b67)


# Wiki Wiki Wiki

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/18337deb-72f1-43fe-b184-277effb735bb)

Searched for ips `128.125.52.138` edited articles on wikipedia and I found this article
> There are 3600 photos =))

``` 
https://en.wikipedia.org/w/index.php?oldid=676540540

```

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/59365fb5-a2fc-41fd-a981-517ff38f78e2)

`bronco{cNi76bV2IVERlh97hP}`



















