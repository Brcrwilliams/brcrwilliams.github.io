---
layout: resource
title: "Learning Resources For Students & Novices"
permalink: /resources/learning
show_title: true
date: 2017-10-23
update_date:
tags: resources
---
## How Do I Get Started In Information Security?

This is a question I get asked every time I talk to college students. A lot of people want to learn about InfoSec, but they have no idea how. The answer for most people is: You have to teach yourself. Even for experienced professionals, InfoSec is about constant self-teaching. This is the nature of the industry. When it comes to information security, defense is all about being one step ahead of the offense; This is why companies hack themselves, and why whitehats are always researching new exploits.

Below is a list of resources that you can use to teach yourself about InfoSec. If there's not a link, just Google it. While this might not sound helpful, I've learned about a lot of things just by hearing a term for the first time and then reading the Wikipedia article for it. If you notice anything wrong with this page, feel free to [contact me](/contact) or [open an issue on GitHub](https://github.com/Brcrwilliams/brcrwilliams.github.io/issues/new).

### Web Application Vulnerabilities

* [SQL Injection \(SQLi\)](https://www.owasp.org/index.php/SQL_Injection)
* [Cross Site Scripting \(XSS\)](<https://www.owasp.org/index.php/Cross-site_Scripting_(XSS)>) - See [XSS Games](#xss-games) for practice
* [Cross Site Request Forgery \(CSRF\)](<https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)>)
* [Server Side Request Forgery \(SSRF\)](https://www.hackerone.com/blog-How-To-Server-Side-Request-Forgery-SSRF)
* [Insecure Direct Object References](<https://www.owasp.org/index.php/Testing_for_Insecure_Direct_Object_References_(OTG-AUTHZ-004)>)
* [Path Traversal](https://en.wikipedia.org/wiki/Directory_traversal_attack)
* [XML External Entity Injection \(XXE\)](https://blog.bugcrowd.com/advice-from-a-researcher-xxe/)
* [OS Command Injection](https://www.owasp.org/index.php/Command_Injection)
* [Buffer Overflow](https://en.wikipedia.org/wiki/Buffer_overflow) - More common in desktop / server software than web applications

### Cryptography  
This section is WIP, but decent explanations for all of these can be found by Googling.

* [Journey Into Cryptography](https://www.khanacademy.org/computing/computer-science/cryptography) - Khan Academy course, highly recommended that you start here.
* [AES cipher explained in comic form](http://www.moserware.com/2009/09/stick-figure-guide-to-advanced.html)
* Known- / Chosen- / Adaptive Chosen Plaintext Attacks
* Known- / Chosen- / Adaptive Chosen Ciphertext Attacks
* Brute Force Attack
* Side-Channel Attack
* Frequency Analysis \(Applies to classical ciphers\)
* Collision Attack
* Birthday Attack
* Padding Oracle Attack
* Replay Attack


### Hashing and Password Storage

* [Hashing + Salting](https://crackstation.net/hashing-security.htm)
* [Rainbow Tables](http://kestas.kuliukas.com/RainbowTables/)
* [Dictionary Attack](https://blog.codinghorror.com/dictionary-attacks-101/)
* [Preventing bad passwords](https://www.usenix.org/conference/usenixsecurity16/technical-sessions/presentation/wheeler) - By the creators of [zxcvbn](https://github.com/dropbox/zxcvbn) \(Scroll down to the video\)

### Penetration Testing

* [Windows Privilege Escalation](http://www.fuzzysecurity.com/tutorials/16.html)
* [Linux Privilege Escalation Cheatsheet](https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/)

### Challenges

The best way to truly learn something is to practice it once you have an idea of the basic concepts. Making mistakes and learning how and why something works or doesn't work is key to gaining a deeper understanding of technical concepts. Below are some challenges that you may use to test your knowledge and (hopefully) learn new things.

* [Over the Wire: Bandit](http://overthewire.org/wargames/bandit/) - Highly recommended for beginners - Basic Bash / Linux usage and basic security concepts
* [Under The Wire](http://www.underthewire.tech/wargames.htm) - Similar to Bandit, but will teach you about Windows PowerShell
* [SQLNinja](http://leettime.net/sqlninja.com/) - Basic SQL Injection
* [Over the Wire: Natas](http://overthewire.org/wargames/natas/) - Web Application Security - Higher level challenges involve Command Injection, SQL Injection, and scripting
* [Over the Wire: Krypton](http://overthewire.org/wargames/krypton/) - Cryptography / Classical Ciphers
* [Hack This Site](https://www.hackthissite.org/) - CTF-style Challenges
* [Hack The Box](https://www.hackthebox.eu/) - General Penetration Testing

<a id="xss-games"></a>
### XSS Games

There are so many of these that they deserve their own section. Most of these challenges are great for beginners and a good way to learn about injection and filter evasion.
* [XSS Challenge (yamagata21)](https://xss-quiz.int21h.jp/)
* [XSS Game (appspot)](https://xss-game.appspot.com/)
* [alert(1) to win](https://alf.nu/alert1)
* [prompt(1) to win](http://prompt.ml/0) \(Very difficult\)
* [cure53's XSS Challenge List](https://github.com/cure53/XSSChallengeWiki/wiki)
