# TryHackMe — Valenfind Hints

Room: https://tryhackme.com/room/lafb2026e10

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/Love%20at%20First%20Breach%202026/Valenfind.md

## Challenge

**Valenfind**

> My Dearest Hacker,
> There’s this new dating app called “Valenfind” that just popped up out of nowhere. I hear the creator only learned to code this year; surely this must be vibe-coded. Can you exploit it?

Goal: What is the flag?

---

## Stage 1 — Initial Recon

<details>
<summary><strong>Hint 1</strong></summary>

Register an account and explore the site after logging in.

Look closely at user profile functionality.

</details>

<details>
<summary><strong>Hint 2</strong></summary>

Profiles let you change the theme.

Watch the browser Network tab while doing that.

</details>

<details>
<summary><strong>Hint 3</strong></summary>

A request is made to fetch a layout file.

Try changing the file name in that request.

</details>

---

## Stage 2 — Finding the Vulnerability

<details>
<summary><strong>Hint 1</strong></summary>

The site appears to load files based on a request parameter.

Think about what happens if you supply your own file path.

</details>

<details>
<summary><strong>Hint 2</strong></summary>

Try reading a common Linux file such as `/etc/passwd`.

</details>

<details>
<summary><strong>Hint 3</strong></summary>

If that works, you have Local File Inclusion.

</details>

---

## Stage 3

<details>
<summary><strong>Hint 1</strong></summary>

Try requesting a file that does not exist.

</details>

<details>
<summary><strong>Hint 2</strong></summary>

The error message reveals part of the filesystem path used by the app.

</details>

<details>
<summary><strong>Hint 3</strong></summary>

The application is located under `/opt/Valenfind/`.

</details>

---

## Stage 4

<details>
<summary><strong>Hint 1</strong></summary>

Check the response headers from the site. Knowing webserver type gives LFI targets  

</details>

<details>
<summary><strong>Hint 2</strong></summary>

The headers reveal the app is running with Werkzeug and Python.

That strongly suggests Flask.

</details>

<details>
<summary><strong>Hint 3</strong></summary>

Once you know it is likely Flask, think about the file names commonly used by small Flask apps.

</details>

<details>
<summary><strong>Hint 4</strong></summary>

`app.py`

</details>

---

## Stage 5

<details>
<summary><strong>Hint 1</strong></summary>

Use the LFI to read files inside `/opt/Valenfind/`.

Start with likely Python application files.

</details>

<details>
<summary><strong>Hint 2</strong></summary>

Try reading `app.py`.

</details>

<details>
<summary><strong>Hint 3</strong></summary>

The source code contains both a hidden admin route and the value needed to access it.

</details>

---

## Stage 6 

<details>
<summary><strong>Hint 1</strong></summary>

Review the source for:

- admin routes
- hardcoded keys
- custom headers
- database export functionality

</details>

<details>
<summary><strong>Hint 2</strong></summary>

There is an admin endpoint that exports the database, and it checks for a custom header token.

</details>

<details>
<summary><strong>Hint 3</strong></summary>

Call the admin export endpoint with the token found in the source, then search the response for `THM{`.

</details>

---
