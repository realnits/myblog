---
author: Nithissh S
pubDatetime: 2024-12-03T15:22:00Z
modDatetime: 2024-12-03T09:12:47.400Z
title: Server-side template injection with information disclosure via user-supplied objects
slug: ssti-portswigger-lab-5
featured: false
draft: false
tags:
  - BSCP
  - SSTI
description:
  This lab is vulnerable to server-side template injection due to the way an object is being passed into the template. This vulnerability can be exploited to access sensitive data. To solve the lab, steal and submit the framework's secret key. You can log in to your own account using the following credentials `content-manager:C0nt3ntM4n4g3r`
---

## Objective 

This lab is vulnerable to server-side template injection due to the way an object is being passed into the template. This vulnerability can be exploited to access sensitive data.

To solve the lab, steal and submit the framework's secret key.

You can log in to your own account using the following credentials: `content-manager:C0nt3ntM4n4g3r`

## Solution 

Just like the previous lab, which we saw where we can edit the product template and the same scenario appears here.. Now can fuzz it by pasting the following expression value `${{<%[%'"}}%\.` on to it and click on preview

Ah, Error response shows that we are using `django` 

![](../../assets/images/bscp/ssti/ssti-15.png)

Normally, the secret key for django exists under settings and in order to get that value, we can use the following payload `{% raw %}{{settings.SECRET_KEY}}{% endraw %}` which will reveal the secret key value of framework 

![](../../assets/images/bscp/ssti/ssti-16.png)

Now submit the value as solution and that solves the lab 

![](../../assets/images/bscp/ssti/ssti-17.png)