# Lab 01 - Reflected XSS into HTML Context with Nothing Encoded

## Lab Information

* **Lab:** Reflected XSS into HTML context with nothing encoded
* **Difficulty:** Apprentice
* **Status:** ✅ Solved

---

# Objective

Exploit the reflected Cross-Site Scripting (XSS) vulnerability in the search functionality to execute JavaScript in the victim's browser.

---

# Tools Used

* Web Browser
* Burp Suite (Optional)

---

# Steps

## 1. Open the Lab

Navigate to the lab and locate the search functionality.

**Screenshot**

![Search Page](screenshots/lab-01-01-search-page.png)

---

## 2. Inject the XSS Payload

Enter the following payload into the search box:

```html
<script>alert(1)</script>
```

Click **Search**.

The payload is reflected directly into the HTML response without any encoding, causing the browser to execute the JavaScript and display an alert dialog.

**Screenshot**

![XSS Executed](screenshots/lab-01-02-xss-alert.png)

---

# Payload Used

```html
<script>alert(1)</script>
```

---

# Why It Works

The application reflects user input directly into the HTML page without performing output encoding or input validation.

As a result, the browser interprets the injected `<script>` tag as executable JavaScript instead of plain text.

---

# Impact

* Execution of arbitrary JavaScript
* Session hijacking
* Cookie theft
* Phishing attacks
* Defacement of web pages
* User impersonation

---

# Prevention

* Encode user input before rendering it in HTML.
* Validate and sanitize all user input.
* Implement a strong Content Security Policy (CSP).
* Use secure templating frameworks with automatic output encoding.

---

# Key Takeaways

* Reflected XSS occurs when user input is immediately reflected in the server response.
* Unencoded HTML output allows attackers to inject executable JavaScript.
* Proper output encoding is the primary defense against reflected XSS.

