---
layout: post
title:  "CSRF briefly"
date:   2016-01-16 16:57:38
categories: security csrf
comments: true
---

### Problem
Let's imagine that person goes to a bank site and logs in. At the same time this person recives spam email with malicious link and clicks it. Browser opens the page, which has hidden form which gets posted via javascript to the bank site. As soon as user has valid cookies from being logged in, malicious post request treated as legit by the bank. And if the request is to withdraw money from your account you're doomed.

### Solution
To use securely generated token (csrf token in Rails) which gets embedded in every form. As soon as attacker doesn't know secret key which is used to generate the token, you're fine by checking token every time you recieve a post request.

### Links
[Anatomy of a Cross-site Request Forgery Attack](http://haacked.com/archive/2009/04/02/anatomy-of-csrf-attack.aspx/)