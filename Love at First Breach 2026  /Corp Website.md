# Romance & Co

Room: https://tryhackme.com/room/lafb2026e7

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/Love%20at%20First%20Breach%202026/Corp%20Website.md

---

# Scenario

**Romance & Co** are preparing for their busiest time of year, Valentine’s Day.
Unfortunately, security alerts indicate the company may already have been compromised.

As a security analyst, your task is to retrace the attacker’s steps and determine how the web application was exploited.

---

## Hint 1

<details>
<summary><strong>Reveal Hint</strong></summary>

Start with **basic reconnaissance of the website**.

Look at:

* Developer tools
* Network requests
* Framework indicators
* Unusual ports the application is running on

</details>

---

## Hint 2

<details>
<summary><strong>Reveal Hint</strong></summary>

Try identifying the technologies used by the website.

Tools that may help:

* Wappalyzer
* BuiltWith
* Inspecting headers
* Looking for framework-specific artifacts

</details>

---

## Hint 3

<details>
<summary><strong>Reveal Hint</strong></summary>

Notice the web server is running on a **different port than typical Python web apps**.

This may indicate a **JavaScript framework server** rather than Flask or Django.

</details>

---

## Hint 4

<details>
<summary><strong>Reveal Hint</strong></summary>

Look for evidence of **React-based frameworks**.

Some frameworks expose identifying headers or client-side artifacts that reveal the exact framework being used.

</details>

---

## Hint 5

<details>
<summary><strong>Reveal Hint</strong></summary>

Once you identify the framework and version, search for **public vulnerabilities**.

Example searches:

```
framework_name version CVE
```

or

```
framework_name RCE exploit
```

</details>

---

## Hint 6, if still stuck for 5

<details>
<summary><strong>Still stuck? Reveal Hint</strong></summary>

There is a known vulnerability allowing **remote command execution through a server component interface**.

Look for public proof-of-concept exploits.

A GitHub search for the CVE will help.

</details>

---

## Hint 7, if still stuck from 5,6

<details>
<summary><strong>Still stuck? Reveal Hint</strong></summary>

Try looking for **other TryHackMe rooms involving this framework**.

These rooms often demonstrate how the exploit works and how to trigger it.

This can provide guidance on **how to use the vulnerability correctly**.

</details>

---

## Hint 8

<details>
<summary><strong>Reveal Hint</strong></summary>

Use the **payload provided in the GitHub exploit repository** for this vulnerability.
If you have not found the well-known GitHub repository, find the related THM room and extract the payload from there.

You will likely need to:

* Send the payload through **Burp Repeater**
* Update the **target IP and port**
* Ensure the request is sent to the correct service port

</details>

---

## Hint 9

<details>
<summary><strong>Reveal Hint</strong></summary>

Once you confirm command execution, try spawning a **reverse shell**.

Remember to:

* Start a listener
* Replace the **attacker IP** with your own machine

</details>

---

## Hint 10

<details>
<summary><strong>Reveal Hint</strong></summary>

Check what commands the current user can run with sudo.

</details>

---

## Hint 11

<details>
<summary><strong>Reveal Hint</strong></summary>

If a binary can be executed with sudo, search for it on **GTFOBins**.

However, the command shown on GTFOBins may need to be **modified to suit the exact `sudo -l` permissions** shown on the target system.

Adjust the command so it runs through the **allowed binary path listed in `sudo -l`**.

</details>


