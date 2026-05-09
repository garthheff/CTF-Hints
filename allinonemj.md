# All in One

Room: https://tryhackme.com/room/allinonemj

This is a fun box where you will get to exploit the system in several ways. Few intended and unintended paths to getting user and root access.

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/allinonemj.md

---

## Initial Enumeration

<details>
<summary>Hint 1</summary>

Start by identifying all open TCP services. There are not many, so each one matters.

</details>

<details>
<summary>Hint 2</summary>

One service allows anonymous access, but do not assume access means write access.

</details>

<details>
<summary>Hint 3</summary>

The web service has more than just the default page. Directory discovery is useful here.

</details>

---

## Web Discovery

<details>
<summary>Hint 1</summary>

Look for a WordPress install under the web root.

</details>

<details>
<summary>Hint 2</summary>

Enumerate the WordPress version, users, themes, and plugins. The plugin list is more useful than the WordPress core version.

</details>

<details>
<summary>Hint 3</summary>

There is also a small non-WordPress page on the root site. Its source contains a clue.

</details>

---

## The Small Page

<details>
<summary>Hint 1</summary>

Read the HTML source carefully, not just the visible text.

</details>

<details>
<summary>Hint 2</summary>

The visible word that sounds like a smell is a hint toward a cipher.

</details>

<details>
<summary>Hint 3</summary>

The hidden text gives both the encrypted value and the key.

</details>

<details>
<summary>Hint 4</summary>

Decode the hidden value using the cipher hinted by the visible text. The result is related to WordPress access.

</details>

---

## WordPress Access

<details>
<summary>Hint 1</summary>

The WordPress username can be discovered through enumeration.

</details>

<details>
<summary>Hint 2</summary>

The decoded clue is close to the password, but pay attention to which part is actually used.

</details>

<details>
<summary>Hint 3</summary>

If the browser acts strangely, check whether cookies or redirects are causing the problem. The credentials may still be accepted even if the login page appears again.

</details>

---

## Plugin Vulnerability

<details>
<summary>Hint 1</summary>

The useful plugin is not one of the default WordPress components.

</details>

<details>
<summary>Hint 2</summary>

The vulnerable plugin exposes a file parameter that can read local files.

</details>

<details>
<summary>Hint 3</summary>

Start by proving file read with a standard Linux file.

</details>

<details>
<summary>Hint 4</summary>

Reading PHP files directly may return nothing because they execute. Use a PHP wrapper to read source instead.

</details>

<details>
<summary>Hint 5</summary>

The WordPress configuration file contains database credentials. These help explain the earlier password clue.

</details>

---

## Getting Code Execution

<details>
<summary>Hint 1</summary>

Once inside WordPress, look for an admin feature that allows editing PHP files.

</details>

<details>
<summary>Hint 2</summary>

The active theme is a good place to make a small, controlled change.

</details>

<details>
<summary>Hint 3</summary>

Test command execution with a harmless identity check before attempting a reverse shell.

</details>

<details>
<summary>Hint 4</summary>

When triggering a reverse shell through a URL, remember that special shell characters need encoding.

</details>

---

## Moving from Web User to Elyana

<details>
<summary>Hint 1</summary>

After getting a shell, check the user home directory. One readable file tells you what to look for next.

</details>

<details>
<summary>Hint 2</summary>

The user flag is not readable as the web user.

</details>

<details>
<summary>Hint 3</summary>

Search for files owned by the target user that the web user can read.

</details>

<details>
<summary>Hint 4</summary>

The useful file is hidden in a system configuration location, not in the user’s home directory.

</details>

<details>
<summary>Hint 5</summary>

Once you find the user password, switch to that user and read the user flag.

</details>

---

## Privilege Escalation

<details>
<summary>Hint 1</summary>

After becoming the user, check sudo permissions.

</details>

<details>
<summary>Hint 2</summary>

The allowed binary can connect streams and execute programs.

</details>

<details>
<summary>Hint 3</summary>

GTFOBins has a direct technique for this binary when it can be run with sudo.

</details>

<details>
<summary>Hint 4</summary>

The root flag is encoded. Decode it before submitting.

</details>

---

