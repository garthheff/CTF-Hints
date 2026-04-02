# CyberLens – Hints

---

## 🌐 Web Enumeration

<details>
<summary>The page does more than what you see</summary>

Uploading a file gives you output, but what’s actually happening behind the scenes?

Open your browser developer tools and watch closely when you upload something.

Where is the request really going?

</details>

---

## 🔍 Follow the Request

<details>
<summary>That port looks interesting</summary>

If you see a request being sent to a different port, that’s usually not by accident.

Try visiting that service directly in your browser.

Does it reveal anything about what it is or how it works?

</details>

---

## 🧠 Understanding the Service

<details>
<summary>It tells you exactly what it is</summary>

Some services are kind enough to introduce themselves.

If you know what the service is, what’s the next logical step?

Think about known vulnerabilities or common attack paths.

</details>

---

## 💣 Initial Access

<details>
<summary>Sometimes you don’t need to reinvent the wheel</summary>

If you’ve identified the service and version, consider tools that already have modules for it.

This is a great time to check frameworks that include prebuilt exploits.

</details>

---

## 🖥️ Post Exploitation

<details>
<summary>You have a shell… now what?</summary>

Don’t rush straight into privilege escalation.

Take a moment to understand where you are and what access you have.

Where do users usually keep important files?

</details>

---

## 🔑 User Flag

<details>
<summary>Think like a user</summary>

If you landed on a Windows machine, where would a standard user store files they care about?

You’re looking for something obvious, not hidden.

</details>

---

## 📈 Privilege Escalation

<details>
<summary>Tools can save you time</summary>

Manual enumeration is great, but it’s easy to miss things.

There are well-known tools designed specifically to highlight privilege escalation paths on Windows.

If you use one, make sure it actually runs on the target system.

</details>


---

## 🚨 Misconfiguration

<details>
<summary>This one stands out</summary>

Look through the results carefully.

You’re searching for something that allows software to run with higher privileges than the current user.

Some settings are far more dangerous than they first appear.

</details>

---

## 💣 Turning Access into SYSTEM

<details>
<summary>Abuse what the system already trusts</summary>

If the system is configured to elevate certain types of executions, you can take advantage of that.

Think about what file types Windows treats as installers.

</details>

---

## 👑 Final Step

<details>
<summary>Did your privileges change?</summary>

Always confirm your current user context.

If successful, you should now have the highest level of access.

</details>

---

## 🏁 Root Flag

<details>
<summary>Same approach, different user</summary>

With elevated access, revisit the same locations as before — but under a more privileged account.

</details>

---
