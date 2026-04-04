# Dav

Room: https://tryhackme.com/room/bsidesgtdav

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/bsidesgtdav.md

---

# Scenario

boot2root machine for FIT and bsides guatemala CTF

---

## Hint 1 – Initial
<details>
<summary>Click to expand</summary>

Start with basic port enumeration.

</details>

---

## Hint 2
<details>
<summary>Click to expand</summary>

Run directory enumeration.

Does anything stand out that matches the room name?

</details>

---

## Hint 3 
<details>
<summary>Click to expand</summary>

A 401 response usually means basic authentication is required.

</details>

---

## Hint 4 
<details>
<summary>Click to expand</summary>

Before brute forcing, think smarter.

Are there common default credentials for this type of service?

Try searching for known defaults.

</details>

---

## Hint 5 – Accessing the Service
<details>
<summary>Click to expand</summary>

Is there a tool designed specifically for WebDAV interaction?

</details>

---

## Hint 6
<details>
<summary>Click to expand</summary>

If you can upload files, what kind of file would give you remote code execution?

Think about web shells.

</details>

---

## Hint 7
<details>
<summary>Click to expand</summary>

Once you have a shell, enumerate the system.

Check home directories.

</details>

---

## Hint 9 – Privilege Escalation
<details>
<summary>Click to expand</summary>

Always run:

sudo -l

Look for commands that can be run as root without a password.

</details>

---

## Hint 10 
<details>
<summary>Click to expand</summary>

If you can run a command as root, you might not need a full exploit.

Can you directly read the root flag?

</details>

---
