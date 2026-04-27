#  Library

Room: https://tryhackme.com/room/bsidesgtlibrary

Library is a boot2root machine created for FIT and BSides Guatemala CTF. The goal is to enumerate exposed services, gain an initial shell, then escalate privileges to root.

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/bsidesgtlibrary.md

------------------------------

# Library Hints

Room: https://tryhackme.com/room/bsidesgtlibrary

Use these hints one at a time. Try not to open the next hint unless you are properly stuck.

---

## User

<details>
<summary>Hint 1</summary>

Start with basic service enumeration. You are looking for the exposed services and their versions.

</details>

<details>
<summary>Hint 2</summary>

There are only a couple of open ports. One gives you a web surface, and the other becomes useful once you have credentials.

</details>

<details>
<summary>Hint 3</summary>

Do not forget to check common web files.

</details>

<details>
<summary>Hint 4</summary>

One common web file gives a very strong hint about which password list to use.

</details>

<details>
<summary>Hint 5</summary>

The hint is not a directory. Think about the value shown in the `User-agent` line.

</details>

<details>
<summary>Hint 6</summary>

You need a username and a password list for SSH.

</details>

<details>
<summary>Hint 7</summary>

The username is related to the room theme.

</details>

<details>
<summary>Hint 8</summary>

Try SSH password testing with the hinted wordlist and the discovered or guessed username.

</details>

<details>
<summary>Hint 9</summary>

Hydra can test one SSH username against a password list.

</details>

<details>
<summary>Hint 10</summary>

Once you have a valid SSH login, check the user’s home directory.

</details>

---

## Root

<details>
<summary>Hint 1</summary>

After getting a shell, always check what the user can run with elevated privileges.

</details>

<details>
<summary>Hint 2</summary>

Look closely at the sudo rule. It does not allow every command, but it allows one script to run as root.

</details>

<details>
<summary>Hint 3</summary>

Read the script. Focus on what files it reads and where it writes the result.

</details>

<details>
<summary>Hint 4</summary>

The script creates a backup archive of part of the web directory.

</details>

<details>
<summary>Hint 5</summary>

Check the permissions under the web root. You cannot write everywhere, but one directory is writable by the current user.

</details>

<details>
<summary>Hint 6</summary>

If a root-run script reads files from a directory you can write to, you may be able to influence what it reads.

</details>

<details>
<summary>Hint 7</summary>

Think about symbolic links.

</details>

<details>
<summary>Hint 8</summary>

Create a symbolic link inside the writable web directory that points to a file only root can normally read.

</details>

<details>
<summary>Hint 9</summary>

Run the allowed backup script through sudo, then inspect the generated archive.

</details>

<details>
<summary>Hint 10</summary>

You do not need an interactive root shell. You only need to make the backup archive include the target file’s contents.

</details>

---

## Extra Nudge

<details>
<summary>Hint 1</summary>

The backup script uses Python’s `zipfile` module and walks through `/var/www/html`.

</details>

<details>
<summary>Hint 2</summary>

The writable location is under the blog directory.

</details>

<details>
<summary>Hint 3</summary>

After creating your link and running the backup script, use archive listing or archive printing tools to inspect the generated zip.

</details>

