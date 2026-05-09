# Cyborg

Room: https://tryhackme.com/room/cyborgt8

Walthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/cyborgt8.md

A box involving encrypted archives, source code analysis and more.

---

## Starting Point

<details>
<summary>Hint 1</summary>

Begin with a full port scan. There are only a small number of open services, so focus on identifying versions and exposed web content.

</details>

<details>
<summary>Hint 2</summary>

One of the web paths exposes configuration-style files. Think about what files might reveal how authentication is handled.

</details>

<details>
<summary>Hint 3</summary>

Look for web-accessible configuration under an `etc`-style path. The interesting service is related to proxy authentication.

</details>

---

## Web Enumeration

<details>
<summary>Hint 1</summary>

The web server leaks a configuration file that points to another file containing credentials.

</details>

<details>
<summary>Hint 2</summary>

The credential file contains a username and a hash rather than a cleartext password.

</details>

<details>
<summary>Hint 3</summary>

The hash format starts with `$apr1$`, which is commonly associated with Apache MD5 style password hashes.

</details>

---

## Cracking

<details>
<summary>Hint 1</summary>

Use a common password list against the discovered hash.

</details>

<details>
<summary>Hint 2</summary>

Make sure the cracking tool is using the correct mode for Apache `$apr1$` MD5.

</details>

<details>
<summary>Hint 3</summary>

The cracked value is not directly an SSH password at this stage. Use it with the archive you find next.

</details>

---

## Archive

<details>
<summary>Hint 1</summary>

After downloading the archive, extract the outer container first.

</details>

<details>
<summary>Hint 2</summary>

The extracted files may not look like normal user files at first. Check the README and file names.

</details>

<details>
<summary>Hint 3</summary>

The extracted directory is a BorgBackup repository. The cracked password unlocks it.

</details>

---

## Backup Repository

<details>
<summary>Hint 1</summary>

List the backup repository to discover the archive name inside it.

</details>

<details>
<summary>Hint 2</summary>

After extracting the Borg archive, inspect the recovered home directory carefully.

</details>

<details>
<summary>Hint 3</summary>

A note in a user's Documents folder contains credentials for a real system user.

</details>

---

## User Access

<details>
<summary>Hint 1</summary>

Use the recovered user credentials to access the target over the remote login service discovered during enumeration.

</details>

<details>
<summary>Hint 2</summary>

Once logged in, check the user's home directory for the user flag.

</details>

<details>
<summary>Hint 3</summary>

Also inspect the recovered bash history from the backup. It contains context, but the main privilege escalation route is easier to find locally after login.

</details>

---

## Privilege Escalation

<details>
<summary>Hint 1</summary>

Check what the user can run with elevated privileges.

</details>

<details>
<summary>Hint 2</summary>

The allowed script lives under an mp3 backup directory.

</details>

<details>
<summary>Hint 3</summary>

Read the script carefully. It accepts an option that stores user-controlled input as a command.

</details>

<details>
<summary>Hint 4</summary>

The script runs that supplied value while executing with elevated privileges.

</details>

<details>
<summary>Hint 5</summary>

An interactive shell may behave strangely because of how the script captures command output. Use the script for a side effect instead of relying on a clean interactive shell immediately.

</details>

<details>
<summary>Hint 6</summary>

One reliable side effect is changing a shell binary so it can later be launched with elevated effective privileges.

</details>

---

## Root

<details>
<summary>Hint 1</summary>

After gaining a stable elevated shell, move to the root user's home directory.

</details>

<details>
<summary>Hint 2</summary>

The root flag is stored in the usual file name.

</details>
