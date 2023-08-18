---
title: Some HackerOne CTF Challenges
date: 2023-04-18 21:18:00 +07:00
categories: [CTF, Web]
tags: [HackerOne CTF]
---

# HackerOne CTF: A Beginner's Perspective
Over the past few weeks, I've been working through various challenges and learning a lot about different attack vectors and techniques. In this blog post, I'll be sharing some of the challenges I tackled and the methods I used to solve them. So, whether you're a seasoned cybersecurity pro or just getting started in the field, I hope you'll find something of interest in my experiences with the HackerOne CTF!

## A little something to get you started
In this challenge, we are given a webpage with the content 'Welcome to level 0. Enjoy your stay.' . Nothing interesting, let's take a look at the source code.

![Sourcecode](/assets/img/hackeronectf/A_little_something_to_get_you_started/1.png)

There's an interesting file in the source code called background.png . Let's make a request to /background.png

![Flag](/assets/img/hackeronectf/A_little_something_to_get_you_started/2.png)
## Micro-CMS v1
In this challenge, we are given a markdown site generator and two samples pages. For this challenge, there are 4 flags.

### Flag 0
Create a new page by clicking 'Create a new page' link. Fill in the title and the content with any value just to see how it works. You'll see that in the address it becomes /page/\<number\>. Try to change the number from 0 to 10. I got a forbidden response at number 7. There could be secret content at this page number.
![Forbidden](/assets/img/hackeronectf/Micro-CMS-v1/Flag0/1.png)
To read the content of this page, we are going to use another endpoint when editing the page. Let's go back to your created page and edit the page, and then change the number to 7.
![Flag0](/assets/img/hackeronectf/Micro-CMS-v1/Flag0/2.png)

### Flag 1
We know that our input is reflected back on the page. This could lead to XSS. We know that we can use Markdown format in the body, but scripts are not allowed. Therefore, the title is our testing point for XSS. I use `blah<script>alert(1)</script>` as the payload.

![XSSPayload](/assets/img/hackeronectf/Micro-CMS-v1/Flag1/1.png)

Save it then go back to home.
![Flag1](/assets/img/hackeronectf/Micro-CMS-v1/Flag1/2.png)

### Flag 2
To get the flag, just add `'` after /page/edit/ .
![Flag2](/assets/img/hackeronectf/Micro-CMS-v1/Flag2/1.png)

### Flag 3
We know that there's a XSS filter at the body, we can bypass it using following payload:
* `<button onclick=alert(1)>blah</button>`
* `<svg onload=alert(1)>`
* `<img src="" onerror=alert(1)>`
* `<iframe onload=alert(1)>`
  
That's some payload that works for me. There should be more payload that works. After planting the payload and executing it, we didn't get a flag. Let's take a look at the source code.
![Flag3](/assets/img/hackeronectf/Micro-CMS-v1/Flag3/1.png)
