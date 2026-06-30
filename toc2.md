# toc2

It's a setup... Can you get the flags in time? 

room: https://tryhackme.com/room/toc2

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/medium/toc2.md

---

<details>
<summary>Hint 1</summary>

Start with a full TCP port scan and pay close attention to service versions and web script output.

</details>

---

<details>
<summary>Hint 2</summary>

The web server does not show the real application immediately. Check common web discovery files that may reveal hidden paths.

</details>

---

<details>
<summary>Hint 3</summary>

The public maintenance page leaks information that is useful later. Read the page text carefully, not just the title.

</details>

---

<details>
<summary>Hint 4</summary>

The hidden path points to an unfinished CMS setup. The room expects you to interact with the installer, not just the finished application.

</details>

---

<details>
<summary>Hint 5</summary>

The CMS version in the installer is old and has a known installer-based vulnerability. Search for the CMS name, exact version, and “installer RCE”.

</details>

---

<details>
<summary>Hint 6</summary>

The vulnerability requires valid database details. The page content and hidden web note provide what you need to complete or abuse the install process.

</details>

---

<details>
<summary>Hint 7</summary>

The vulnerable installer step involves a configuration value that gets written into the CMS config file. This can be abused to place executable code into the config.

</details>

---

<details>
<summary>Hint 8</summary>

You can either reproduce the exploit manually with an intercepting proxy or use a public PoC for the specific CVE. Make sure the target path matches the CMS directory, not just the web root.

</details>

---

<details>
<summary>Hint 9</summary>

After getting command execution, upgrade it into a reverse shell and begin local enumeration as the web server user.

</details>

---

<details>
<summary>Hint 10</summary>

Look in user home directories. One user has a directory that strongly hints at a path toward root access.

</details>

---

<details>
<summary>Hint 11</summary>

Inside that directory, inspect file permissions. One executable has elevated privileges, and its source code is available.

</details>

---

<details>
<summary>Hint 12</summary>

The source code performs a permission check, waits briefly, and then opens the file afterward. That gap is the weakness.

</details>

---

<details>
<summary>Hint 13</summary>

This is a Time-of-Check to Time-of-Use issue. You need one file path to appear safe during the check, then point somewhere more valuable before the file is actually opened.

</details>

---

<details>
<summary>Hint 14</summary>

A symlink race is the intended technique. Point the argument at a readable file first, then swap it to the protected credential backup during the program’s delay.

</details>

---

<details>
<summary>Hint 15</summary>

Avoid using `/tmp` if the race keeps failing. Modern Linux protections and sticky directories can interfere with privileged symlink tricks.

</details>

---

<details>
<summary>Hint 16</summary>

Use a writable, non-sticky directory controlled by the web user. The CMS web directory worked during our solve.

</details>

---

<details>
<summary>Hint 17</summary>

A controlled timing loop works better than two very fast loops. Start the privileged program while the link points to the harmless file, wait a short fraction of a second, then swap the link to the protected file.

</details>

---

<details>
<summary>Hint 18</summary>

If the timing does not hit immediately, increase the delay gradually between attempts. The vulnerable program gives you a generous window.

</details>

---

<details>
<summary>Hint 19</summary>

The protected file contains credentials for a more privileged account. Use them with an interactive shell to switch user.

</details>

---

<details>
<summary>Hint 20</summary>

Once you become root, the final flag is in the usual root location.

</details>
