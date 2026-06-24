# Lab 01 - Reflected Cross-Site Scripting (XSS)

## Lab Information

**Platform:** PortSwigger Web Security Academy

**Category:** Cross-Site Scripting (XSS)

**Difficulty:** Apprentice

---

## Objective

Identify and exploit a reflected Cross-Site Scripting (XSS) vulnerability within a search functionality.

---

## Vulnerability Overview

Reflected Cross-Site Scripting occurs when user-supplied input is immediately reflected in the server response without proper sanitization or output encoding.

An attacker can craft malicious URLs containing JavaScript payloads which execute when a victim visits the page.

---

## Testing Methodology

1. Accessed the application's search functionality.
2. Submitted test input to identify reflection points.
3. Confirmed that user input was reflected directly within the HTML response.
4. Injected a JavaScript payload to verify code execution.

---

## Payload Used

```html
<script>alert(1)</script>
```

---

## Result

The application reflected the supplied payload without sanitization.

The JavaScript payload executed successfully within the browser, confirming the presence of a reflected XSS vulnerability.

---

## Impact

Successful exploitation may allow an attacker to:

* Execute arbitrary JavaScript
* Steal session tokens
* Perform actions on behalf of users
* Deface web pages
* Deliver phishing content

---

## Remediation

* Apply output encoding before rendering user input.
* Validate and sanitize user-controlled data.
* Implement a Content Security Policy (CSP).
* Follow secure coding practices for user input handling.

---

## Learning Outcome

This lab demonstrated how reflected XSS vulnerabilities occur when user input is embedded into HTML responses without proper encoding. It reinforced the importance of input validation and output encoding in web applications.
