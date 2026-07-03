# Why does the tool's browser not open?

# Fix: Burp Suite Render "Could not connect to browser" (Ubuntu)

**Platform:** Ubuntu Linux  
**Burp Suite:** 2023–2025  

---

## Problem

In Burp Suite on Ubuntu, the **Render** tab may display:

```
Could not connect to browser
```

The embedded Burp browser (Chromium) fails to launch, preventing HTML rendering.

---

## Root Cause

Burp Suite uses a bundled Chromium sandbox located at:

```
~/.BurpSuite/burpbrowser/<version>/chrome-sandbox
```

On Ubuntu, Chromium requires the sandbox binary to have:

- owner: `root`
- permission: `4755` (setuid)

If these permissions are missing or altered, Chromium will refuse to start, causing the Burp Render tab to fail.

This commonly occurs after:

- Burp Suite updates  
- Manual extraction or file copy  
- Permission changes  
- Ubuntu upgrades  

---

## Fix

Navigate to the Burp browser directory:

```bash
cd ~/.BurpSuite/burpbrowser
ls
```

Open the version directory (example):

```bash
cd 145.0.7632.45
```

Apply correct sandbox permissions:

```bash
sudo chown root:root chrome-sandbox
sudo chmod 4755 chrome-sandbox
```

---

## Result

Restart Burp Suite → **Render tab works correctly**.

---
