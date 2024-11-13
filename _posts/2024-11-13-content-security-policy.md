---
layout: post
title: Content Security Policy
date: 2024-11-13 21:55 +0530
author: "Gaurav Kumar"
tags: "browser theory security"
categories: "clean_code"
---

Content Security Policy (CSP) is a security feature that helps prevent various types of attacks, such as `Cross-Site Scripting (XSS)` and data injection attacks, by controlling the sources from which resources like scripts, images, and fonts can be loaded.

Let us understand this using a simple HTML.

Here is a simple HTML which does not use Content-Security-Policy. This will load the required font and use that for the h1 header.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Hello, World!</title>
  </head>
  <body>
    <h1 style="font-family: 'Roboto', sans-serif;">
      Hello, this is a test without CSP!
    </h1>

    <!-- Using Google Fonts without CSP -->
    <link
      href="https://fonts.googleapis.com/css2?family=Roboto&display=swap"
      rel="stylesheet"
    />
  </body>
</html>
```

The below HTML has a Content-Security-Policy which will block this font:

You'd see the below error in browser console logs:

> Refused to load the stylesheet 'https://fonts.googleapis.com/css2?family=Roboto&display=swap' because it violates the following Content Security Policy directive: "default-src 'self'". Note that 'style-src-elem' was not explicitly set, so 'default-src' is used as a fallback.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Hello, World!</title>
    <!-- Content Security Policy (CSP) -->
    <meta
      http-equiv="Content-Security-Policy"
      content="default-src 'self'; font-src 'self' https://fonts.gstatic.com;"
    />
  </head>
  <body>
    <h1 style="font-family: 'Roboto', sans-serif;">
      Hello, this is a test without CSP!
    </h1>

    <!-- Using Google Fonts without CSP -->
    <link
      href="https://fonts.googleapis.com/css2?family=Roboto&display=swap"
      rel="stylesheet"
    />
  </body>
</html>
```

### `http-equiv="Content-Security-Policy"`

- The `http-equiv` attribute tells the browser to treat the content as if it were an HTTP header. In this case, the `Content-Security-Policy` directive is used to define the CSP.
- Instead of setting the CSP in an HTTP response header, it's set within the HTML document itself via a `<meta>` tag.

### `content="default-src 'self'; font-src 'self' https://fonts.gstatic.com;"`

This is the actual Content Security Policy being applied. It consists of two directives: `default-src` and `font-src`.

#### - `default-src 'self';`

- This sets the default source for all resources (e.g., scripts, images, media, etc.) to `'self'`.
- The `'self'` keyword means that resources can only be loaded from the same origin as the current page (i.e., the same protocol, domain, and port).
- This is a security measure to prevent loading resources from external sources that might not be trusted.

#### - `font-src 'self' https://fonts.gstatic.com;`

- This is a more specific rule that applies only to fonts.
- The `'self'` keyword means that fonts can only be loaded from the same origin as the page.
- `https://fonts.gstatic.com` explicitly allows fonts to be loaded from this specific domain, which is a part of Google Fonts. This allows you to safely load fonts from Google Fonts while restricting other font sources.
- Without this rule, the browser would block any attempt to load fonts from external sources, which could break the appearance of your page if you rely on an external font (e.g., Google Fonts).

---

So, with this CSP directive added, it will block the font from `https://fonts.googleapis.com`.

To fix this, we can update the CSP to allow the fonts from this domain also.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Hello, World!</title>
    <!-- Content Security Policy (CSP) -->
    <meta
      http-equiv="Content-Security-Policy"
      content="default-src 'self'; font-src 'self' https://fonts.gstatic.com https://fonts.googleapis.com;"
    />
  </head>
  <body>
    <h1 style="font-family: 'Roboto', sans-serif;">
      Hello, this is a test without CSP!
    </h1>

    <!-- Using Google Fonts without CSP -->
    <link
      href="https://fonts.googleapis.com/css2?family=Roboto&display=swap"
      rel="stylesheet"
    />
  </body>
</html>
```

The above HTML does look okay, but there is another issue. We will still see the following error in the console logs of the browser:

> Refused to apply inline style because it violates the following Content Security Policy directive: "default-src 'self'". Either the 'unsafe-inline' keyword, a hash ('sha256-xxxxx='), or a nonce ('nonce-...') is required to enable inline execution. Note that hashes do not apply to event handlers, style attributes and javascript: navigations unless the 'unsafe-hashes' keyword is present. Note also that 'style-src' was not explicitly set, so 'default-src' is used as a fallback.

This happens because the CSP's `default-src 'self'` does not allow inline styles or inline scripts by default. Inline styles need an exception in the `style-src` directive or by allowing `'unsafe-inline'`.

Let's do that to fix the issue to make it work:

```html
<meta
  http-equiv="Content-Security-Policy"
  content="
          default-src 'self';
          style-src 'self' 'unsafe-inline' https://fonts.googleapis.com;
          font-src 'self' https://fonts.gstatic.com;
      "
/>
```

> Using 'unsafe-inline' in a Content Security Policy (CSP) is generally not recommended from a security perspective. When you allow 'unsafe-inline', it allows scripts to be executed directly from the HTML, making it easier for malicious scripts to execute. This is one of the most common vectors for XSS attacks.

To avoid using `unsafe-inline` parameter, we can externalize the CSS file.

CSS File:

```css
.custom-header {
  font-family: "Roboto", sans-serif;
}
```

HTML File:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Hello, World!</title>
    <!-- Content Security Policy (CSP) -->
    <meta
      http-equiv="Content-Security-Policy"
      content="
              default-src 'self';
              style-src 'self' https://fonts.googleapis.com;
              font-src 'self' https://fonts.gstatic.com;
          "
    />
    <!-- External stylesheet for custom styling -->
    <link rel="stylesheet" href="style.css" />
    <!-- Google Fonts -->
    <link
      href="https://fonts.googleapis.com/css2?family=Roboto&display=swap"
      rel="stylesheet"
    />
  </head>
  <body>
    <h1 class="custom-header">Hello, this is a test without CSP!</h1>
  </body>
</html>
```

The following YouTube video provides a good explanation about using CSP:
[Content Security Policy explained](https://www.youtube.com/watch?v=txHc4zk6w3s)
