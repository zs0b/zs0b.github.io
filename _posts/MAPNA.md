# MAPNA CTF WriteUP

<picture>
 <source media="(prefers-color-scheme: dark)" srcset="/assets/MAPNA/img/MAPNA.png">
 <source media="(prefers-color-scheme: light)" srcset="/assets/MAPNA/img/MAPNA.png">
 <img alt="YOUR-ALT-TEXT" src="/assets/MAPNA/img/MAPNA.png">
</picture>

Hello comrades, I'm glad you came here to read my article about **MAPNA CTF**

Let's start now

## PLC I 

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/8171acf1-48fb-4ded-9e1c-0615ed3e868d)

I opened **plc.pcap** and read it once, what caught my eye was the form flag lol `1: MAPNA{y`

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/ba532bd6-b1e1-49a4-b5d2-7e658b49f718)

Ok then I understand, just search and arrange in order :_)

`MAPNA{y0u_sHOuLd_4lW4yS__CaR3__PaADd1n9!!}`


## What next?

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/b57d2094-de91-4c17-b68b-8871644698bc)
```
from Crypto.Util.number import long_to_bytes

def decrypt(enc, KEY):
    m = enc ^ KEY
    return long_to_bytes(m).decode('utf-8')

# Given values
KEY = 23226475334448992634882677537728533150528705952262010830460862502359965393545
enc = 2290064970177041546889165766737348623235283630135906565145883208626788551598431732

# Decrypt the flag
flag = decrypt(enc, KEY)

print("Decrypted flag:", flag)
```
![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/62930a68-0b8c-4fd7-b63d-b870d930282b)

`MAPNA{R_U_MT19937_PRNG_Predictor?}`

## Flag Holding

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/5d7a7abc-0951-408e-9b94-be4962a2ccff)

Now let's use Burp Suite to intercept the request from the web, then throw it through the Repeater

And here, the topic says you are not from this address `http://flagland.internal/`

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/0cbb8b97-4961-4d5a-9c6b-eef4cfdce9c0)


Then we will add the header `Referer:http://flagland.internal/`

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/e295e38c-19ab-4098-ac5d-4ee2a0784e42)

And it says `Unspecified "secret"`

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/622d5b5a-7819-4eb9-af0c-be397a6377b5)

So we just need to add param "secret" to the request
and because this is a GET request, the url will be inserted

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/89025755-b63c-40fd-acf1-84b2df6f1d1f)

We have received the next hint secret is the protocol that both the server and browser use to communicate, so we add http

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/206a9c6c-f82d-45b2-a907-c55f8e9127bf)

Noo !!!

The problem says there is no accept GET but FLAG, so let's change GET to FLAG @~@

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/f44ccec3-3b04-4ec4-bf0d-16c90f7eb01f)

`MAPNA{533m5-l1k3-y0u-kn0w-h77p-1836a2f}`

goodbye, thank you for reading until now //~//
