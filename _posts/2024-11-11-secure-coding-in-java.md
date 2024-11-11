---
layout: post
title: Secure Coding in Java - OWASP TOP 10
date: 2024-11-11 17:20 +0530
author: "Gaurav Kumar"
tags: "java theory important"
categories: "clean_code"
---

## OWASP TOP 10 VULNERABILITIES

### BROKEN ACCESSS CONTROL

Broken access control allows users to access resources or functionalities they shouldn't. This happens when the application fails to check user permissions.

Example: A non-admin user can access admin pages by simply changing the URL.

```java
public void viewAdminPage(HttpServletRequest request, HttpServletResponse response) throws IOException {
    HttpSession session = request.getSession();
    // No validation of user role
    response.getWriter().println("Welcome to the admin page!");
}
```

Without checking the user’s role, anyone with a session can access this page.

```java
public void viewAdminPage(HttpServletRequest request, HttpServletResponse response) throws IOException {
    HttpSession session = request.getSession();
    if ("admin".equals(session.getAttribute("role"))) {
        response.getWriter().println("Welcome to the admin page!");
    } else {
        response.sendError(HttpServletResponse.SC_FORBIDDEN, "Access denied");
    }
}
```

Best Practices:

- Implement role-based access control.
- Ensure all restricted resources check user permissions.
- Use server-side access checks, not just client-side.

### CRYPTOGRAPHIC FAILURES

Cryptographic failures happen when sensitive data is not protected properly. This includes unencrypted data transmission or storing passwords in plaintext.

Example: Storing passwords in plain text allows anyone with access to the database to see them.

```java
public void storePassword(String password) {
    // Storing plain text password
    database.save("password", password);
}
```

Storing passwords in plain text exposes users to risks if the database is compromised.

```java
public String hashPassword(String password) throws NoSuchAlgorithmException {
    MessageDigest md = MessageDigest.getInstance("SHA-256");
    byte[] hash = md.digest(password.getBytes(StandardCharsets.UTF_8));
    return Base64.getEncoder().encodeToString(hash);
}
```

Best Practices:

- Use strong algorithms like AES for data encryption.
- Never store passwords in plain text; use salted hashing (like bcrypt).
- Avoid using deprecated or weak algorithms.

### INJECTION

Injection vulnerabilities occur when untrusted data is executed as part of a query or command. SQL injection is common, where an attacker can manipulate database queries.

Example:

An attacker can inject SQL code like `"' OR '1'='1"` into username to bypass authentication.

```java
public boolean authenticate(String username, String password) throws SQLException {
    String query = "SELECT * FROM users WHERE username='" + username + "' AND password='" + password + "'";
    Statement stmt = connection.createStatement();
    ResultSet rs = stmt.executeQuery(query);
    return rs.next();
}
```

Avoid constructing queries by concatenating strings with user inputs.

```java
public boolean authenticate(String username, String password) throws SQLException {
    String query = "SELECT * FROM users WHERE username=? AND password=?";
    PreparedStatement stmt = connection.prepareStatement(query);
    stmt.setString(1, username);
    stmt.setString(2, password);
    ResultSet rs = stmt.executeQuery();
    return rs.next();
}
```

Best Practices:

- Use prepared statements or parameterized queries.
- Validate and sanitize all user inputs.
- Avoid constructing queries by concatenating strings with user inputs.

### INSECURE DESIGN

This occurs when an application is designed without considering security, such as lack of validation, logging, or rate limiting.

Example:

Allowing any file to be uploaded without checking file type or content.

```java
public void uploadFile(MultipartFile file) throws IOException {
    // No validation of file type or size
    Files.copy(file.getInputStream(), Paths.get("/uploads/" + file.getOriginalFilename()));
}
```

An attacker could upload malicious files, leading to server compromise.

```java
public void uploadFile(MultipartFile file) throws IOException {
    String filename = file.getOriginalFilename();
    if (filename.endsWith(".jpg") || filename.endsWith(".png")) {
        Files.copy(file.getInputStream(), Paths.get("/uploads/" + filename));
    } else {
        throw new IOException("Invalid file type");
    }
}
```

Best Practices:

- Validate file type, size, and content before processing uploads.
- Apply rate limiting and input validation throughout the application.
- Regularly review and test your application’s design for security gaps.

### SECURITY MISCONFIGURATION

Security misconfigurations arise when the system is improperly configured, such as leaving debug mode enabled in production.

Example:

Showing detailed error messages to users.

```java
public void doGet(HttpServletRequest request, HttpServletResponse response) {
    try {
        // Potential error-causing code
    } catch (Exception e) {
        response.getWriter().println("Error: " + e.getMessage());  // Insecure
    }
}
```

Revealing error messages can expose sensitive system information to attackers.

```java
public void doGet(HttpServletRequest request, HttpServletResponse response) {
    try {
        // Potential error-causing code
    } catch (Exception e) {
        response.sendError(HttpServletResponse.SC_INTERNAL_SERVER_ERROR, "An unexpected error occurred.");
    }
}
```

Best Practices:

- Disable detailed error messages in production.
- Regularly review and update server configurations.
- Disable unnecessary features, modules, or services.

### VULNERABLE AND OUTDATED COMPONENTS

Using outdated libraries or components can expose applications to known vulnerabilities.

Example:

Using an old library version without checking for security updates.

Old versions may contain known vulnerabilities that attackers can exploit.

Best Practices:

- Regularly update libraries and dependencies.
- Use vulnerability scanning tools to monitor dependencies.

### IDENTIFICATION AND AUTHENTICATION FAILURES

Weak authentication mechanisms can allow unauthorized access to the application.

Example:

Using a hardcoded password for authentication.

```java
public boolean authenticate(String username, String password) {
    return "password123".equals(password);  // Hardcoded, weak password
}
```

Hardcoded passwords are easily guessable and cannot be changed dynamically.

```java
public boolean authenticate(String username, String password) {
    String hashedPassword = fetchPasswordFromDatabase(username);
    return BCrypt.checkpw(password, hashedPassword);  // Using strong hashing
}
```

Best Practices:

- Enforce strong password policies.
- Use multi-factor authentication (MFA) for sensitive actions.
  Avoid hardcoding sensitive data.

### SOFTWARE AND DATA INTEGRITY FAILURES

Allowing unverified data (like updates) opens up the risk of malicious content.

Example:

Downloading files from untrusted sources without verification.

```java
public void downloadFile(String url) throws IOException {
    URL downloadUrl = new URL(url);
    try (InputStream in = downloadUrl.openStream()) {
        Files.copy(in, Paths.get("downloaded-file.zip"));
    }
}
```

The server may download and run malicious files from unverified sources.

Best Practices:

- Verify sources before downloading and executing files.
- Use signed code and validate checksums for data integrity.

### SECURITY LOGGING AND MONITORING FAILURES

Insufficient logging and monitoring allow attacks to go undetected.

Example:

Not logging failed login attempts.

```java
public void login(String username, String password) {
    if (authenticate(username, password)) {
        System.out.println("Login successful.");
    }
}
```

No logging makes it hard to detect brute force attempts or suspicious behavior.

```java
public void login(String username, String password) {
    if (authenticate(username, password)) {
        System.out.println("Login successful.");
    } else {
        System.out.println("Login failed for user: " + username);
    }
}
```

Best Practices:

- Log important security events.
- Regularly review logs and use alerting for suspicious activity.

### SERVER-SIDE REQUEST FORGERY (SSRF)

SSRF vulnerabilities occur when an application lets users provide URLs or addresses to which the server will make requests without validating them. This can lead to unauthorized access to internal services or sensitive data exposure.

Example:

Allowing users to fetch data from any URL without verifying if the target is trusted or necessary.

```java
public void fetchData(String url) throws IOException {
    URL target = new URL(url);
    try (InputStream in = target.openStream()) {
        // Process data
    }
}
```

Attackers can specify internal or protected endpoints to gather information or cause unintended effects. Without validation, the server could end up making requests to internal or even harmful services.

Validate URLs to ensure requests only go to trusted external servers.

```java
public void fetchData(String url) throws IOException {
    // Allowing only trusted domains
    if (!url.startsWith("https://trusted-domain.com")) {
        throw new SecurityException("Untrusted URL blocked");
    }
    URL target = new URL(url);
    try (InputStream in = target.openStream()) {
        // Process data securely
    }
}
```

Best Practices:

- Validate and restrict URLs to a set of trusted domains or patterns.
- Block requests to internal IP addresses, localhost, or sensitive endpoints.
- Use allow-lists and network-level controls where possible to limit where requests can go.
