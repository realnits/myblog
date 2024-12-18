---
author: Nithissh S
pubDatetime: 2024-09-07T15:22:00Z
modDatetime: 2024-09-07T09:12:47.400Z
title: SQL injection UNION attack, determining the number of columns returned by the query
slug: sqli-portswigger-lab-3
featured: false
draft: false
tags:
  - BSCP
  - SQL Injection
description:
  This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response, so you can use a UNION attack to retrieve data from other tables. The first step of such an attack is to determine the number of columns that are being returned by the query. You will then use this technique in subsequent labs to construct the full attack. To solve the lab, determine the number of columns returned by the query by performing a SQL injection UNION attack that returns an additional row containing null values.
---

## Objective 

This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response, so you can use a UNION attack to retrieve data from other tables. The first step of such an attack is to determine the number of columns that are being returned by the query. You will then use this technique in subsequent labs to construct the full attack.

To solve the lab, determine the number of columns returned by the query by performing a SQL injection UNION attack that returns an additional row containing null values.

## Solution 

After spinning up the lab, this is how the application looks like kind of ecommerce application 

![](../../assets/images/bscp/sqli/sqli1.png)

In the page which is shown, I've gone through a product category filter and in this case, I've clicked on the `Corporate Gifts` and added `'` at the end of the URL and recieved a internal server meaning our SQL query got broke and app may not able to process further 

![](../../assets/images/bscp/sqli/sqli2.png)

With the following payload `'UNION+SELECT+NULL--` will check if there is only one column exists but still we face `500` meaning there is more than 1

![](../../assets/images/bscp/sqli/sqli3.png)

Adding other `NULL` to check for two column but still `500`

![](../../assets/images/bscp/sqli/sqli4.png)

But when we add the third null value which responded with `200` status code, we can see that there are three columns exists with the following payload `'UNION+SELECT+NULL,NULL,NULL--` 

![](../../assets/images/bscp/sqli/sqli5.png)