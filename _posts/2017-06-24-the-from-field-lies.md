---
layout:     post
title:      "The \"From\" field is lying to you"
date:       2017-06-24
summary:    "Have you ever gotten an email that looked suspicious, but since the domain that it came from was correct you decided to trust it? Don't do that."
---

Today, I took a dive into my old email inbox. I never check this email anymore, but since it's tied to some of my oldest accounts I still have to log into it occasionally. Taking a look at my inbox, I saw a few emails from Amazon saying that I cancelled an order with them. This is odd, because I never placed any orders for these products. In fact, I never even use the Amazon account that is tied to this email. I use the Amazon account that I registered with my .edu email in college for that sweet student discount.

![The email in question](/images/1/emailbody.png)

The email is from order-update@amazon.com. Seems legit, right? Well, when mousing over the links it becomes apparent that they don't go to Amazon at all.

![Mouseover image](/images/1/mouseover.png)

This is weird, because the email says that it's from Amazon. Doesn't that mean it's from Amazon? Apparently not. Let's take a look at the email headers and see if they can tell us anything. On outlook.com, you can do this by clicking the arrow next to reply and choosing "View message source." Gmail is the same, but you must choose "Show original."

![Viewing the headers](/images/1/view-source.png)

Looking at the email headers, we can see who _really_ sent the email. This one appears to be from a "gxkr@hillwood.com."

![Email headers](/images/1/return-path-header.png)

The email protocol doesn't have any mechanism to verify or prevent the sender from altering the from header. Therefore, it's pretty easy to spoof the address that the email appears to be from. Additionally, since there's several legitimate uses for changing the from header, most email clients don't show or warn the user when it has been tampered with. This makes it easy for phishers to trick people who assume that the from address is trustworthy.

Since curiosity got the better of me, I decided to go ahead and visit the link in the email and see what these guys are up to. Just in case the site tried to exploit some XSS or CSRF vulnerabilities (doubtful), I cleared out all my browser data before clicking the link. I was redirected to the following page:

_![Malicious site](/images/1/malicious-site.png)_

A virus, how scary. Fortunately, the internet doesn't work like that and you can't get viruses just from browsing to sketchy websites (although I would still recommend against visiting them). Clearly, the people controlling this web page expect me to call "Windows support" who will then remote into my computer to "fix" it. I'm not sure if they could help me though, because I'm using a Mac.
