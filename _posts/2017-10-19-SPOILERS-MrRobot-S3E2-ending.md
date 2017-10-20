---
layout:     post
title:      "[Spoilers] Mr. Robot S3E2 Ending Explained"
date:       2017-10-19
summary:    "Mr. Robot S3E2 ending has a hidden puzzle."
---

In the ending of episode 2 Agent DiPierro realizes that Elliot knew he was being spied on, and sent a message to them. How?

![The email](/images/3/email.png)

You can actually visit the link in the email that he sent: [https://sandbox.vflsruxm.net/plans.rar](https://sandbox.vflsruxm.net/plans.rar)

This page has some base64.
```
UmFyIRoHAQDz4YLrCwEFBwAGAQGAgIAAaqQxujQCAwvdBwSpCSDmLtDfgAMAGGpC
b3VhcUs5UjhqWHhmcEU2a0dWLnBuZwoDAuhpGqmXR9MBykrZA0BlRWMiPzYAaWtG
4y5CKWrcagjRpWw+CVQlthXGUgSkhQFSEhBGwaFVq2EkwkYSBZ6Zla40lwY0rRaF
YNLMIiQT4VjFG+hgzEBJRKSWJUkjMwKs+JICQKJIEmY5LLcuW9n5fczubmbvebzu
83e/od3nd3m973dzeeec5mefmqzezsyKE+oJECBAomSmJ6C7VNf855mq/uDUU3vp
4Dq97Nz5F/BaAle0uC8p5xgnMS0+g+9yy3BrKldbq+D5tF18/XHvSipzd2CSz2qa
buoI+K3HqzKb8us7zREMzox1pxL3fDuZSJ/WjIHmVg5n3GB8J49zsgrIpCs3o2Fw
elehOp1zb+sPUBNrznGIbSYOKAZ9UXYnXzIr+eO7tbYlRR4HZT1ZQf4cVQpoG8PS
EZMt+g1q0Oe9/0uAZ8X05JXXez3sxGm3+uFtQK6+Hto065Ku72KlNiPKTQIeyT5l
crgkTqA4NF1QqPR7OXBmUrhWxcrzIaRqr/WBQXahXUrJtkXnJpgsd0wEboBRIbP4
sfItuGqV+KqWOblf5Ot64Hvy6jXyUEMgiDY51dmWVjyMKv5G5LdwZyyIJLKbIri1
C6Te+w/dZRNa+LP+Nt0x/ZZdyAlVWR3jJ/scC6msO/Q+4WmJ7n6Kzk80AobYK4pI
DOwx2Uei8FCZS/JFYrCthCdW4T4VpcoPaJZY3lI7Rfx6qhjsmRWpLoFK0b7aLR9l
YfKROnNI885Mp2ywOKkoSxboKOJlVp7WVGwTsTQcNxc7vKbP/yEEPbVJRb8ZdbtE
PVMYEWp6/CnHuhT7N1CWQP+lLHLU3LKw5+NWfyakyxF4hXHJAHMZc2t0UdKNrA8m
p7FsXvA/shbhioeagOCmZTIkFWPjXHwylCrUYlP8AOPrfEJur1n4og/+/HL/c6Y5
pksv3JtvDwlx6u78GsXPu7kmlFPzO1sO/p9Lt0FKp9MgDSZdvauPtO9lWFIJfHp7
/8N08CseGojeBhTMJQokVvyljS7QLP20EevozBIfJAZ+C7fFYeeaDvOmkqvrU0ip
jgIWnUv/IJbAhKH3Cr+jg5aUhW4UTHd3Q0wdnNq4afNttSRwaJuX7MGjFTdewGM9
LbYN/wIRSwib3+M2mGuywZ92YVVFpjZ4vMhNMSw9eLUFaMhfDXMnUe2M51XusaVF
qiQCyf63UDCPCQ8P0bfElu/UvS2+NyX0ALh+pSx9dxrjn3tmO9htRYwHwu5ebeXR
ZD6p9CyMpd4is9eDfl9+IspNGtGMaOntIJiBc8RzmvR4oOxlK3jP9ZJ4Rc+Qu4hx
k+O/EeVJkZ2Y6hDg/OAdd1ZRAwUEAA==
```

When decoded, when can see that this is actually a rar file.  
![Decoded rar](/images/3/decode.png)

So, we can decode the base64, write it to a file, then unzip the file.  

![Writing file](/images/3/write.png)

![Unzipping file](/images/3/unrar.png)

We get a .png file with a QR code.

![QR code](/images/3/jBouaqK9R8jXxfpE6kGV.png)

When scanning the QR code, we get a link to this github: [https://github.com/RedBalloonShenanigans/MonitorDarkly](https://github.com/RedBalloonShenanigans/MonitorDarkly)

It's the exploit that they used on Elliot's monitor!  
Once Darlene left, Elliot loaded up Kali Linux and checked for RATs (Remote Access Tools) installed on his machine. This didn't find anything, because the FBI actually attacked the firmware on his monitor instead of his computer. This allowed them to see what he was doing while simultaneously remaining undetected - or so they thought.

Elliot figured out that they had used an exploit on his monitor. So, he devised this clever email. When DiPerro says "it's for us," she had just deciphered the email link and realized that Elliot knew about their monitor exploit.
