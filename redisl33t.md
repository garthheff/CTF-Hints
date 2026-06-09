# Red

The match has started, and Red has taken the lead on you.
But you are Blue, and only you can take Red down.

However, Red has implemented some defense mechanisms that will make the battle a bit difficult:
1. Red has been known to kick adversaries out of the machine. Is there a way around it?
2. Red likes to change adversaries' passwords but tends to keep them relatively the same. 
3. Red likes to taunt adversaries in order to throw off their focus. Keep your mind sharp!

This is a unique battle, and if you feel up to the challenge. Then by all means go for it!

A classic battle for the ages.

Room: https://tryhackme.com/room/redisl33t

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/redisl33t.md

-----------------

## Initial Enumeration

<details>
<summary>Hint 1</summary>

Start with a full TCP port scan.

</details>

<details>
<summary>Hint 2</summary>

There are only two useful services exposed.

</details>

<details>
<summary>Hint 3</summary>

The web service is the intended starting point.

</details>

---

## Web Enumeration

<details>
<summary>Hint 1</summary>

Look closely at the URL after visiting the site.

</details>

<details>
<summary>Hint 2</summary>

The application loads pages through a query parameter.

</details>

<details>
<summary>Hint 3</summary>

Directory brute forcing mostly shows static template files, but one PHP file controls how they are loaded.

</details>

---

## Source Disclosure

<details>
<summary>Hint 1</summary>

Try to read the PHP source instead of only testing for directories.

</details>

<details>
<summary>Hint 2</summary>

The parameter accepts values that start with a lowercase letter.

</details>

<details>
<summary>Hint 3</summary>

PHP stream wrappers can help bypass this restriction.

</details>

<details>
<summary>Hint 4</summary>

Use `php://filter` to Base64 encode files before reading them.

</details>

---

## Local File Read

<details>
<summary>Hint 1</summary>

Once file read works, read common Linux files.

</details>

<details>
<summary>Hint 2</summary>

Look for local users with interactive shells.

</details>

<details>
<summary>Hint 3</summary>

The important users match the room theme.

</details>

---

## Getting Credentials

<details>
<summary>Hint 1</summary>

Inspect hidden files in the first user’s home directory.

</details>

<details>
<summary>Hint 2</summary>

Shell history reveals how a password list was created.

</details>

<details>
<summary>Hint 3</summary>

A hidden reminder file provides the base word.

</details>

<details>
<summary>Hint 4</summary>

Use the same Hashcat rule mentioned in the shell history.

</details>

<details>
<summary>Hint 5</summary>

If the rule file is not in the expected location, search for it.

</details>

<details>
<summary>Hint 6</summary>

Use the generated list against SSH.

</details>

---

## First Shell

<details>
<summary>Hint 1</summary>

After logging in, do not trust the messages sent to your terminal.

</details>

<details>
<summary>Hint 2</summary>

The Base64 strings are part of the theme, but they are not necessarily useful passwords.

</details>

<details>
<summary>Hint 3</summary>

Failed sudo attempts can cause problems.

</details>

<details>
<summary>Hint 4</summary>

If your password stops working, try another candidate from the generated list.

</details>

---

## Investigating the Interference

<details>
<summary>Hint 1</summary>

Check running processes while the messages appear.

</details>

<details>
<summary>Hint 2</summary>

Look for processes owned by the other themed user.

</details>

<details>
<summary>Hint 3</summary>

The suspicious process repeatedly creates a reverse shell.

</details>

<details>
<summary>Hint 4</summary>

Killing one process is not enough because new ones keep appearing.

</details>

---

## Pivot

<details>
<summary>Hint 1</summary>

Find the hostname used by the recurring callback.

</details>

<details>
<summary>Hint 2</summary>

Check how that hostname resolves locally.

</details>

<details>
<summary>Hint 3</summary>

Inspect `/etc/hosts`.

</details>

<details>
<summary>Hint 4</summary>

If editing the file fails, check its attributes.

</details>

<details>
<summary>Hint 5</summary>

Append-only does not mean read-only.

</details>

<details>
<summary>Hint 6</summary>

Point the callback hostname at your AttackBox and listen on the callback port.

</details>

---

## Second Shell

<details>
<summary>Hint 1</summary>

Wait for the recurring callback to connect.

</details>

<details>
<summary>Hint 2</summary>

Once connected, stabilise the shell.

</details>

<details>
<summary>Hint 3</summary>

Consider adding SSH access for a more stable session.

</details>

---

## Privilege Escalation

<details>
<summary>Hint 1</summary>

Enumerate hidden files and directories in the second user’s home directory.

</details>

<details>
<summary>Hint 2</summary>

One hidden directory contains an unusual binary.

</details>

<details>
<summary>Hint 3</summary>

Compare this binary with the normal system version.

</details>

<details>
<summary>Hint 4</summary>

The useful copy has the SUID bit set.

</details>

<details>
<summary>Hint 5</summary>

Check the version of the binary.

</details>

---

## Exploit Adjustment

<details>
<summary>Hint 1</summary>

A standard exploit may target the wrong binary path.

</details>

<details>
<summary>Hint 2</summary>

The normal system binary is not the useful one.

</details>

<details>
<summary>Hint 3</summary>

The exploit needs to execute the hidden SUID copy.

</details>

<details>
<summary>Hint 4</summary>

One option is to modify the source before compiling.

</details>

<details>
<summary>Hint 5</summary>

If you already have a compiled binary, use a short symlink path and patch the embedded path.

</details>

<details>
<summary>Hint 6</summary>

The replacement path must fit inside the original string length.

</details>

---

## Root

<details>
<summary>Hint 1</summary>

After the exploit succeeds, confirm your user ID.

</details>

<details>
<summary>Hint 2</summary>

The final flag is in root’s home directory, but it is not named `root.txt`.

</details>
