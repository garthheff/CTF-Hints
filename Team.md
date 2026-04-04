
# TryHackMe – Team (Hints Version)

 Beginner friendly boot2root machine

Use these hints progressively. Try not to jump ahead unless you're stuck.

Room: https://tryhackme.com/room/teamcw

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/team.md

---

## 🔹 Hint 1 – Initial Access

<details>
<summary>Where should you start looking?</summary>

There are multiple services exposed, but one stands out as the most common entry point in CTFs.

Focus on HTTP first.

</details>

---

## 🔹 Hint 2 – Website Looks Empty?

<details>
<summary>Why does the site look like a default Apache page?</summary>

The site is using virtual hosts.

Try adding a hostname manually to your hosts file. e.g team.thm

</details>

---

## 🔹 Hint 3 – Enumeration

<details>
<summary>What tool should you use?</summary>

Use a directory brute-forcing tool and include common backup extensions.

Think about files developers might forget to delete.

</details>

---

## 🔹 Hint 4 – First Breakthrough

<details>
<summary>What directory should you investigate further?</summary>

One of the discovered directories contains scripts.

Run another round of enumeration specifically on that directory.

</details>

---

## 🔹 Hint 5 – Credentials

<details>
<summary>What are you looking for in discovered files?</summary>

Look for sensitive information in older or backup files.

Pay attention to anything that looks like credentials.

</details>

---

## 🔹 Hint 6 – FTP

<details>
<summary>Where can you use the credentials?</summary>

One of the exposed services allows authentication using those credentials.

Check what files are available once you log in.

</details>

---

## 🔹 Hint 7 – Important Note

<details>
<summary>What does the internal note reveal?</summary>

There are two key hints:

- A development site exists  
- A private SSH key has been stored somewhere it shouldn’t be  

</details>

---

## 🔹 Hint 8 – Dev Site

<details>
<summary>How do you access the dev site?</summary>

Add another hostname to your hosts file based on the note.

</details>

---

## 🔹 Hint 9 – LFI

<details>
<summary>What vulnerability exists on the dev site?</summary>

Look at how files are loaded via parameters.

Try accessing system files to confirm your suspicion.

</details>

---

## 🔹 Hint 10 – Targeted Enumeration

<details>
<summary>What should you look for using LFI?</summary>

The note told you exactly what to find.

Focus on configuration files containing sensitive key material.

</details>

---

## 🔹 Hint 11 – SSH Access

<details>
<summary>How do you use what you found?</summary>

You now have a private key.

Clean it up and try logging in as a user.

</details>

---

## 🔹 Hint 12 – Privilege Escalation (User 1 → User 2)

<details>
<summary>What should you check after getting a shell?</summary>

Always check sudo permissions.

Look for anything you can run as another user without a password.

</details>

---

## 🔹 Hint 13 – Script Analysis

<details>
<summary>What is wrong with the script?</summary>

It asks for input and then runs it directly.

Think about what happens if you provide something unexpected.

</details>

---

## 🔹 Hint 14 – Reverse Shell

<details>
<summary>How can you turn this into a shell?</summary>

Instead of a simple command, try executing something that connects back to your machine.

</details>

---

## 🔹 Hint 15 – Next Privilege Escalation

<details>
<summary>What should you check as the new user?</summary>

Check group memberships and look for writable files owned by root.

</details>

---

## 🔹 Hint 16 – History Clues

<details>
<summary>Where can you find clues about system activity?</summary>

User command history can reveal scripts, paths, and previous actions.

</details>

---

## 🔹 Hint 17 – Root Script

<details>
<summary>What kind of file are you looking for?</summary>

A script that is:

- Owned by root  
- Writable by your group  

</details>

---

## 🔹 Hint 18 – Final Exploit

<details>
<summary>How do you escalate to root?</summary>

Modify the script so that when it runs, it executes your payload.

</details>

---

## 🔹 Hint 19 – Getting Root

<details>
<summary>How do you receive access?</summary>

Set up a listener and wait for the script to run.

</details>

---

## 🔹 Hint 20 – Final Step

<details>
<summary>Where is the flag?</summary>

Once you have root, check the root user's home directory.

</details>

---

## 🧠 General Advice

- Enumerate thoroughly  
- Don’t skip recursion  
- Always check for backup files  
- Read everything you find carefully  
- Follow the clues logically  
