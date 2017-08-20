---
layout:     post
title:      "Paj's SQL Injection CTF Write-Up"
date:       2017-08-19
summary:    "Discovering SQL injection attack vectors with... Microsoft Excel?"
---

## The Challenge

Last Monday, I went onto [/r/netsec](https://www.reddit.com/r/netsec/) and saw an interesting post. It was titled "SQL Injection CTF with a difference."

![/r/netsec post](/images/2/netsec.png)

When I clicked the link I was brought to [http://sqli-ctf.pajhome.org.uk/](http://sqli-ctf.pajhome.org.uk/). I recognized this url immediately. This domain belongs to a security researcher known as "Paj" and I have been to it before because I've seen several websites use [his MD5 / SHA scripts](http://pajhome.org.uk/crypt/md5/index.html). On this page, I was greeted with an introduction to the challenge.

![Challenge introduction](/images/2/intro.png)

Paj gives us a couple hints. Exploiting the SQL injection vulnerability is easy, but finding it is hard. So, it sounds like the real challenge is finding the attack vector.

Upon starting the challenge we are sent to a page at /quote. It contains a simple form:  
![quote](/images/2/quotepage.png)

I decided to put in some test data and submit it to see what it gives me:  
![input validation](/images/2/quote-test.png)

So, it seems there's some simple input validation in place. I don't know what postcodes in England look like, but thankfully someone in the Reddit thread left a hint. I decided to submit some valid data and see what I get:  
![valid data](/images/2/quote-valid.png)

![risk page](/images/2/quote-result.png)

When I submitted the form it gave me a simple insurance quote, giving an estimated premium. There's also a risk link. This link goes to a page that explains how the risk is calculated.

![risk page](/images/2/riskpage.png)

This page is where I made my biggest mistake with solving the CTF, _I didn't pay attention to what I was reading._ It explains exactly how the risk level of the postcode is determined, albeit in terms that I'm not familiar with. The two links go to gov.uk for [the indices of deprivation data](https://www.gov.uk/government/statistics/english-indices-of-deprivation-2015) and arcgis.com for [the dataset](https://ons.maps.arcgis.com/home/item.html?id=ef72efd6adf64b11a2228f7b3e95deea). I decided to check out the dataset first. What I found was not particularly useful.

![dataset](/images/2/dataset.png)

There's a bunch of codes here, but there's not really anything that I didn't already have. One thing of note is that there are a bunch of postcodes in the first and second columns. I decided to grab a few hundred of them and run them through Burp's Intruder to see if I could get a different response from the form.

![payloads](/images/2/intruder-payloads.png)

I just copy-pasted a bunch of postcodes into the payload list and configured the Intruder to inject on the postcode parameter.

![positions](/images/2/intruder-positions.png)

From the results, I got two different responses.  
{% highlight html %}
<h1>Home contents insurance</h1>

<table>
    <tr><td>Value of contents</td><td>&#163;1000</td></tr>
    <tr><td>Postcode</td><td>HA7 2LF (low <a href="risk">risk</a>)</td></tr>
    <tr><td>Premium</td><td>&#163;10.00</td></tr>

</table>
{% endhighlight %}

{% highlight html %}
<h1>Home contents insurance</h1>

<table>
    <tr><td>Value of contents</td><td>&#163;1000</td></tr>
    <tr><td>Postcode</td><td>HA2 9JL (medium <a href="risk">risk</a>)</td></tr>
    <tr><td>Premium</td><td>&#163;20.00</td></tr>

</table>
{% endhighlight %}

These responses are pretty much the same, except one is low risk and one is medium risk. I hadn't managed to get a high-risk postcode yet, so I started wondering if something different would happen if I managed to find one. The dataset contained over a million postcodes, so I didn't want to try and brute force all of them. This is when I decided to go back to the risk page and figure out how the risk is calculated.

The risk page mentioned some sort of crime rank that is used to determine the risk of the postcode. I started reading through the documents on [the indices of deprivation page](https://www.gov.uk/government/statistics/english-indices-of-deprivation-2015) and found out that the index of multiple deprivation is this rank. This data takes a bunch of LSOA (Lower Layer Super Output Area) codes and ranks them by how deprived the area is. The most deprived area has an index of deprivation of 1, and is what I needed to find. I downloaded the file titled "File 1: index of multiple deprivation" and opened it.

![cover](/images/2/lsoa-explaination.png)

The first page of the document explained what it was, and it was obvious now that I was getting warmer. This document has the deprivation ranks, which is exactly what I'm looking for.

_![data](/images/2/lsoa-sorting.png)_

I flipped over to the next worksheet. Sure enough, the deprivation ranks were there. I selected the whole column and sorted it smallest to largest.

![sorted data](/images/2/lsoa-sorted.png)

After sorting the data, I had my most deprived area. I still didn't have a postcode, so I decided to ask my friend Google if he could give me a postcode for this LSOA. I could have also done a grep on the dataset file, but this was faster.

![google](/images/2/lsoa-to-postcode.png)

Now that I had my postcode. I went back to the quote form and tried it.

![correct data](/images/2/correct-data.png)

Now, when I submitted the form a third option showed up. Bingo! This must be it.

I submitted the form again with an option selected, and now I had an alarm parameter. Now comes the moment of truth. Is it vulnerable? The hints at the beginning of the challenge said it would be easy to exploit, and there wouldn't be any filtering. So, let's stick an ' in it.

![sqli1](/images/2/sqli-1.png)

Success! The SQL syntax error means that it's certainly vulnerable. Now we just have to extract the content of the flag table. We can do this with a union. First I'll try doing `' UNION SELECT * FROM flag;#`. This will work if the flag table only has one column.

![sqli2](/images/2/sqli-2.png)

It worked! The flag select statement was joined with the alarm select statement, and I got the flag.

## The Conclusion

Now, Paj said that the entire purpose of this challenge was to start a discussion on how to reliably detect this kind of issue, so I will offer my opinion. Truth be told, there's only one good way to detect this kind vulnerability automatically, and that's through source code analysis. If you don't have the source code available, then I think manual testing is the only practical way to find this type of vulnerability. After I got invited to the private subreddit, I found that Paj released [the source code of the challenge](https://gist.github.com/paj28/bf0f7718c64c3ff9de195c59e2ae3754). There's a few noteworthy things about it.

First is how the risk is determined:  
{% highlight python %}
def rank2risk(rank):
    if rank > 20000:
        return 'low', 1
    elif rank > 1000:
        return 'medium', 2
    else:
        return 'high', 3
{% endhighlight %}

Only the top 1,000 most deprived LSOAs are considered high-risk, and this is out of approximately 32,000 LSOAs. There's also approximately 1.7 million postcodes in England. It's not practical to try fuzzing with this data while you are also scanning for vulnerabilities _and that's assuming you know what the intended format of the data is to begin with._

Another noteworthy part of the source code is the vulnerability itself (comments added).  
{% highlight python %}
alarm = flask.request.form.get('alarm') # Source
if risk == 'high' and not alarm:
    # Give an error if the risk is high but alarm is blank.
    errors['alarm'] = 'Not answered'

if errors:
    # Show the form if there are errors.
    return flask.render_template('form.html', **locals())

conditions = ''
if risk == 'high':
    if alarm != 'yes':
        # If the alarm is not enabled, increase the premium by 1%
        premium_percent += 1

    # Fetch the conditions from the DB
    with get_connection().cursor() as cursor:
        cursor.execute("select conditions from alarm_conditions where status='%s'" % alarm) # Sink
        for row in cursor.fetchall():
            conditions += row[0] + ' '
{% endhighlight %}

This vulnerability would be easily detected through source code analysis. We can see the source, `alarm = flask.request.form.get('alarm')`, and also the unsafe sink, `cursor.execute("select conditions from alarm_conditions where status='%s'" % alarm)`. The sink is vulnerable to SQL injection because it concatenates the user input to the query string, rather than passing it as a second argument as is done in the postcode query. If there is one thing that this challenge demonstrates, it's the reliability of source code analysis.
