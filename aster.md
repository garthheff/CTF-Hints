# Aster

Hack my server dedicated for building communications applications.

Room: https://tryhackme.com/room/aster

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/medium/aster.md

---

## Hint 1

<details>
<summary>Hint 1</summary>

Start with a full TCP port scan. This room has more going on than just a normal web server.

Look closely at any service names related to communication or telephony.

</details>

---

## Hint 2

<details>
<summary>Hint 2</summary>

The web server contains useful files, but the main attack path is not a typical web exploit.

Enumerate directories and review anything that looks like a script, compiled code, or unusual file type.

</details>

---

## Hint 3

<details>
<summary>Hint 3</summary>

A compiled Python file can often be reversed back into readable source.

Check the Python version first, then use a Python bytecode decompiler.

</details>

---

## Hint 4

<details>
<summary>Hint 4</summary>

The decompiled Python source uses messy variable names and encoded strings.

Do not focus too much on the variable names. Decode the obvious encoded data and look for a username or service hint.

</details>

---

## Hint 5

<details>
<summary>Hint 5</summary>

The decoded Python clue points toward a communication framework installed on the server.

Compare that clue with the open ports from your scan.

</details>

---

## Hint 6

<details>
<summary>Hint 6</summary>

One of the open services accepts structured text commands over TCP.

You cannot just type a username and password directly. The service expects fields such as an action, username, and secret, followed by a blank line.

</details>

---

## Hint 7

<details>
<summary>Hint 7</summary>

The Python clue gives a likely username, but not the secret.

The secret is weak enough to be found with a common password wordlist.

</details>

---

## Hint 8

<details>
<summary>Hint 8</summary>

Once authenticated to the communication service, use its management command feature to enumerate configured users.

Look for users and secrets that might also work for another exposed service.

</details>

---

## Hint 9

<details>
<summary>Hint 9</summary>

A user discovered through the communication service can be reused for remote login.

Be careful with special characters when copying or pasting the password.

</details>

---

## Hint 10

<details>
<summary>Hint 10</summary>

After getting a shell, check the usual local privilege escalation locations.

Cron is especially important in this room.

</details>

---

## Hint 11

<details>
<summary>Hint 11</summary>

A root cron job runs something from a directory you probably cannot read directly.

However, the user’s home directory contains a Java archive that explains what the root-run task is doing.

</details>

---

## Hint 12

<details>
<summary>Hint 12</summary>

List the contents of the Java archive, then inspect the compiled class.

You do not need full Java source to understand it. Java bytecode tools can show hardcoded paths and strings.

</details>

---

## Hint 13

<details>
<summary>Hint 13</summary>

The Java code checks whether a specific trigger file exists in a world-writable location.

If that trigger file exists, the root-run process writes an output file into the user’s home directory.

</details>

---

## Hint 14

<details>
<summary>Hint 14</summary>

Create the trigger file, then wait for the root cron job to run.

Cron jobs may take up to a minute or so.

</details>

---

## Hint 15

<details>
<summary>Hint 15</summary>

The final step does not give you an interactive root shell.

Instead, the root-owned process writes the final content into a file that the normal user can read.

</details>

---

## Technique Summary

<details>
<summary>Hint 16</summary>

The overall route is:

```text
Port scan
→ Web enumeration
→ Reverse Python bytecode
→ Decode hidden clue
→ Authenticate to the management interface
→ Enumerate service users
→ Reuse credentials for remote login
→ Check cron
→ Inspect Java archive
→ Trigger root-run file write
```

</details>
