# Chill Hack

Room: https://tryhackme.com/room/chillhack

Chill the Hack out of the Machine.

Easy level CTF. Capture the flags and have fun!

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/chillhack.md

---

## Initial Enumeration

<details>
<summary>Hint 1</summary>

Start by identifying the exposed services. There are only a few open ports, and each one matters.

</details>

<details>
<summary>Hint 2</summary>

One service allows anonymous access. Do not skip checking the files available there.

</details>

<details>
<summary>Hint 3</summary>

The note from the anonymous service gives you a clue about filtering. Keep that in mind when testing the web application.

</details>

---

## Web Enumeration

<details>
<summary>Hint 1</summary>

Directory brute forcing reveals an interesting hidden web path.

</details>

<details>
<summary>Hint 2</summary>

The hidden page accepts input and appears to run something server-side.

</details>

<details>
<summary>Hint 3</summary>

Simple commands may be filtered, but command chaining can help you understand what is actually happening.

</details>

<details>
<summary>Hint 4</summary>

Once you confirm server-side command execution, aim for a shell as the web server user.

</details>

---

## First Shell

<details>
<summary>Hint 1</summary>

After landing as the web server user, enumerate the home directories.

</details>

<details>
<summary>Hint 2</summary>

One user has a helper script in their home directory. Read it carefully.

</details>

<details>
<summary>Hint 3</summary>

The helper script accepts user input and later treats that input as something executable.

</details>

<details>
<summary>Hint 4</summary>

Check what the web server user is allowed to run with elevated privileges. The answer points back to that helper script.

</details>

---

## User Pivot

<details>
<summary>Hint 1</summary>

Use the allowed sudo rule to run the helper script as the target user.

</details>

<details>
<summary>Hint 2</summary>

When the helper script asks for a message, think about what kind of input would give you an interactive shell.

</details>

<details>
<summary>Hint 3</summary>

Once you become the user, collect the user flag and continue enumerating from that account.

</details>

---

## Web Files and Credentials

<details>
<summary>Hint 1</summary>

The web directory contains another application outside the main visible site.

</details>

<details>
<summary>Hint 2</summary>

Look through the PHP source files for database connection details.

</details>

<details>
<summary>Hint 3</summary>

The database contains user records, but the stored passwords are hashed.

</details>

<details>
<summary>Hint 4</summary>

Cracking the database hashes gives useful context, but those passwords are not necessarily Linux account passwords.

</details>

---

## Hidden File Path

<details>
<summary>Hint 1</summary>

The web app’s image directory contains a suspicious image file.

</details>

<details>
<summary>Hint 2</summary>

Do not only inspect the image visually. Check whether it contains embedded data.

</details>

<details>
<summary>Hint 3</summary>

Extracting from the image gives you an archive.

</details>

<details>
<summary>Hint 4</summary>

The archive is password protected. Use a wordlist-based cracking approach.

</details>

<details>
<summary>Hint 5</summary>

The extracted source code reveals a password after decoding a base64 string.

</details>

---

## Second User Pivot

<details>
<summary>Hint 1</summary>

The decoded password belongs to another local user hinted at throughout the room.

</details>

<details>
<summary>Hint 2</summary>

Use the password to access that user directly rather than trying to reuse the earlier web passwords.

</details>

<details>
<summary>Hint 3</summary>

After logging in as this user, check their groups carefully.

</details>

---

## Root Privilege Escalation

<details>
<summary>Hint 1</summary>

Membership in a certain container-related group is the key to root.

</details>

<details>
<summary>Hint 2</summary>

Check whether any container images are already available locally. Internet access may not be available, so do not rely on pulling new images.

</details>

<details>
<summary>Hint 3</summary>

A local lightweight image is enough. The goal is to mount the host filesystem inside the container.

</details>

<details>
<summary>Hint 4</summary>

Once the host filesystem is mounted, change root into it and read the final proof from the root directory.

</details>

---

## Nudge Summary

<details>
<summary>Open only if you are stuck</summary>

The intended route is:

Anonymous service note → hidden web command page → command injection shell → sudo helper script to become the first user → web source and database enumeration → stego image → cracked ZIP → decoded password → SSH as the second user → Docker group privilege escalation → root.

</details>
