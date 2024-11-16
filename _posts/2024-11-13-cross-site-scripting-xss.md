---
layout: post
title: Cross-Site Scripting (XSS)
date: 2024-11-13 22:49 +0530
author: "Gaurav Kumar"
tags: "browser theory security"
categories: "clean_code"
---

Type of injection attack where malicious scripts are injected in trusted websites, and executed by the visitor's browser.

Let us see a sample to understand a XSS security issue:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Simple XSS Demo</title>
  </head>
  <body>
    <h1>Simple XSS Example</h1>
    <form method="GET" action="">
      <label for="name">Enter your name:</label>
      <input type="text" id="name" name="name" />
      <button type="submit">Submit</button>
    </form>

    <p>
      Hello,
      <span id="output">
        <!-- This is where the vulnerability lies -->
        <?php echo $_GET['name']; ?> </span
      >!
    </p>
  </body>
</html>
```

This code is vulnerable to Cross-Site Scripting (XSS) because it directly inserts user input into the page without checking or cleaning it. When a user submits their name via the form, the name is shown on the page using PHP like this:

```php
<?php echo $_GET['name']; ?>
```

If a user enters a malicious script (For example: `<script>alert('XSS');</script>`), it will be executed in the browser. This could allow attackers to steal cookies, redirect users, or perform other harmful actions.

A good way to practically learn about these attacks, we can make use of the following "insecure" containerized apps:

[DAMN VULNERABLE WEB APPLICATION](https://github.com/digininja/DVWA?tab=readme-ov-file#damn-vulnerable-web-application)

`docker run --rm -it -p 80:80 --name vulnerable vulnerables/web-dvwa`

[OWASP Juice Shop](https://owasp.org/www-project-juice-shop/)

`docker run --rm -p 3000:3000 --name juice-shop bkimminich/juice-shop`

## REFLECTED CSS

This scenario is similar to the previous example, where an attacker can inject and execute JavaScript in a user's browser, taking advantage of client-side vulnerabilities.

For example, a hacker could create a malicious shortened URL like [http://somewebsite/xss_r/?name=%3Cscript%3Ealert%28%22XSS%21%21%21%22%29%3C%2Fscript%3E#](http://somewebsite/xss_r/?name=%3Cscript%3Ealert%28%22XSS%21%21%21%22%29%3C%2Fscript%3E#). If a victim clicks on this link, it will execute the JavaScript on their machine. This can potentially allow the attacker to steal sensitive information, such as cookies, session data, or personal details, by exploiting the vulnerabilities in the user's browser.

![snapshot]({{ site.baseurl }}/assets/img/xss-reflected.png)
