# Red

Hack into a vulnerable database server with an in-memory data-structure in this semi-guided challenge!

Room: https://tryhackme.com/room/res

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/res.md

These hints are written for the room state we encountered. The public intended privilege escalation path appears to be broken on this instance, so the later hints point toward the workaround we used.

---

## Enumeration

<details>
<summary>Hint 1</summary>

Start with a full TCP scan and pay attention to anything that looks like a database service exposed directly to the network.

</details>

<details>
<summary>Hint 2</summary>

There is a web server and a Redis service. The Redis service is more important than it first looks.

</details>

<details>
<summary>Hint 3</summary>

Check whether Redis requires authentication. If it does not, inspect its server information and configuration.

</details>

---

## Redis

<details>
<summary>Hint 1</summary>

Redis can sometimes be abused to write files if configuration changes are allowed.

</details>

<details>
<summary>Hint 2</summary>

Look for a place where writing a file would immediately become useful. The web server gives you a good target.

</details>

<details>
<summary>Hint 3</summary>

Think about writing a small server-side script into the web root, then calling it through the browser.

</details>

---

## Web Shell

<details>
<summary>Hint 1</summary>

Once the file write works, use the web server to execute a simple command and confirm which user you are.

</details>

<details>
<summary>Hint 2</summary>

A basic command execution page is enough to turn this into a reverse shell.

</details>

<details>
<summary>Hint 3</summary>

The first shell lands as the web server user. This is only the first foothold.

</details>

---

## Moving Beyond www-data

<details>
<summary>Hint 1</summary>

The web user does not have much access. Look at the Redis installation and who owns it.

</details>

<details>
<summary>Hint 2</summary>

Redis is running from a user-owned directory. The Redis module header is present on the machine.

</details>

<details>
<summary>Hint 3</summary>

A public Metasploit Redis module path may get part of the way there, but the generated module can fail to load. Building a small module on the target avoids compatibility issues.

</details>

<details>
<summary>Hint 4</summary>

First prove that a harmless Redis module can load. Then think about a module that lets Redis run a system command.

</details>

---

## Getting vianka

<details>
<summary>Hint 1</summary>

Redis can write files, but it cannot create missing directories by itself.

</details>

<details>
<summary>Hint 2</summary>

The usual SSH key trick fails if the target `.ssh` directory does not already exist.

</details>

<details>
<summary>Hint 3</summary>

Once you have command execution through Redis, create the missing SSH directory for the Redis user.

</details>

<details>
<summary>Hint 4</summary>

After the directory exists, return to the Redis file-write trick and place your public key where SSH will trust it.

</details>

---

## Privilege Escalation

<details>
<summary>Hint 1</summary>

Public walkthroughs point to a SUID binary as the intended path. this was broken for us

</details>

<details>
<summary>Hint 2</summary>

Check the intended binary carefully. On our instance, it was not actually SUID.

</details>

<details>
<summary>Hint 3</summary>

The intended path appears broken on this machine. Do not spend forever trying to force it if the permissions do not match the public writeups.

</details>

<details>
<summary>Hint 4</summary>

If the intended SUID file-read path is missing, think about another way to recover the local user's password.

</details>

<details>
<summary>Hint 5</summary>

The password is weak enough to be found from a common wordlist. A local brute-force approach is more practical than trying the whole list over SSH.

</details>

<details>
<summary>Hint 6</summary>

Testing passwords through the local privilege prompt is much faster and cleaner than opening a new SSH connection for every guess.

</details>

<details>
<summary>Hint 7</summary>

Once you find the local user's password, check what that user can do with elevated privileges.

</details>

---

## Broken Room Note

<details>
<summary>Important</summary>

The intended public path is:

Redis file write to web shell → web shell as `www-data` → SUID file-read binary → read shadow → crack `vianka` password → sudo to root.

On our instance, the SUID file-read binary was not SUID, so that step was broken. The workaround was:

Redis module command execution → create SSH directory → add SSH key for `vianka` → brute force or confirm the local password with a common wordlist → use sudo.

</details>

