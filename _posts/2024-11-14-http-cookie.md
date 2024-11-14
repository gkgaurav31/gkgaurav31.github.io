---
layout: post
title: HTTP Cookie
date: 2024-11-14 22:26 +0530
author: "Gaurav Kumar"
tags: "browser theory security"
categories: "clean_code"
---

HTTP is stateless, meaning each request is independent, with no memory of previous requests. For example, if you visit a website and add items to a shopping cart, the server doesn’t remember your cart between page loads. HTTP cookies solve this by storing small pieces of data on your browser, allowing the server to "remember" your state.

[HTTP Cookies by Hussein Nasser](https://www.youtube.com/watch?v=sovAIX4doOE)

## CREATE COOKIE

### CLIENT SIDE

Visit any website such as [https://example.com/](https://example.com/)

Open Developer Console (F12) and go to console. We can create a cookie using console via `document.cookie` method.

Example:

```js
document.cookie = "username=gauk;";
```

We can view the cookie from Application tab of the Developer Console.

![snapshot]({{ site.baseurl }}/assets/img/create-cookie-client-1.png)

### SERVER SIDE

We can create a server which will _tell_ the browser to create a cookie.

Here's sample code using ExpressJS:

```js
const express = require("express");
const cookieParser = require("cookie-parser");

const app = express();
app.use(cookieParser());

app.get("/set-cookie", (req, res) => {
  res.cookie("servercookie", "1");
  res.send("Cookie set");
});

app.get("/get-cookie", (req, res) => {
  res.json(req.cookies);
});

app.listen(3000, () => console.log("Server Started. Listening on Port 3000"));
```

The Server's Response will look like this:

```bash
curl http://localhost:3000/set-cookie -I
HTTP/1.1 200 OK
X-Powered-By: Express
Set-Cookie: servercookie=1; Path=/
Content-Type: text/html; charset=utf-8
Content-Length: 10
ETag: W/"a-n//AkPpkqXeymDlCsW2AfqfgXjc"
Date: Thu, 14 Nov 2024 17:25:31 GMT
Connection: keep-alive
Keep-Alive: timeout=5
```

In the Server Response, we can see a header called `Set-Cookie` which directs the browser to create the cookie with key `servercookie` and value `1`.

## COOKIE PROPERTIES

### SENT WITH EVERY REQUEST

Cookies are sent automatically with each request to the domain that set them, helping the server recognize the client.

### COOKIE SCOPE

1. Domain: Specifies which domain can access the cookie, e.g., `.example.com` allows all subdomains.

   Example:

   `document.cookie = "username=JohnDoe; domain=.example.com";`

2. Path: Limits the cookie to a specific URL path, such as `/account`.

   Example:

   `document.cookie = "username=JohnDoe; path=/account";`

### EXPIRES AND MAX-AGE

- Expires: Sets a fixed expiration date for the cookie.

  Example:

  `document.cookie = "username=JohnDoe; expires=Thu, 18 Dec 2024 12:00:00 UTC";`

- Max-Age: Specifies how long (in seconds) the cookie lasts from the time it is set.

  Example:

  `document.cookie = "username=JohnDoe; max-age=3600";`

### SAME-SITE
