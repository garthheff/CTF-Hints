# Opacity

Opacity is a Boot2Root made for pentesters and cybersecurity enthusiasts.

Opacity is an easy machine that can help you in the penetration testing learning process.

There are 2 hash keys located on the machine (user - local.txt and root - proof.txt). Can you find them and become root?

Hint: There are several ways to perform an action; always analyze the behavior of the application.

Room: https://tryhackme.com/room/opacity

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/opacity.md

-----------------------

## Enumeration

<details>
<summary>Hint 1</summary>

Start with a full TCP scan. The important services are web, SSH, and SMB.

</details>

<details>
<summary>Hint 2</summary>

SMB is worth checking, but do not spend too long there if you only see the default shares.

</details>

<details>
<summary>Hint 3</summary>

The web application has more than just the login page. Look for additional directories.

</details>

<details>
<summary>Hint 4</summary>

Pay close attention to the `/cloud/` path.

</details>

## Web Application

<details>
<summary>Hint 1</summary>

The cloud feature does not upload a local file in the normal multipart way. It asks for a URL.

</details>

<details>
<summary>Hint 2</summary>

Host files from your AttackBox and watch your HTTP server logs to see what the target actually requests.

</details>

<details>
<summary>Hint 3</summary>

The application appears to restrict uploads to images.

</details>

<details>
<summary>Hint 4</summary>

Do not assume that a successful upload means code execution. Test whether the file is executed or served statically.

</details>

<details>
<summary>Hint 5</summary>

Double extensions are useful for testing the filter, but they may still be served as static image files.

</details>

## Initial Access

<details>
<summary>Hint 1</summary>

The extension check is based on the submitted URL.

</details>

<details>
<summary>Hint 2</summary>

Think about parts of a URL that may be seen by the application’s validation but not sent in the actual HTTP request.

</details>

<details>
<summary>Hint 3</summary>

A URL fragment can make a URL appear to end in an image extension while causing the server to fetch a different path.

</details>

<details>
<summary>Hint 4</summary>

Try making the submitted URL look like this pattern:

```text
http://ATTACKBOX:PORT/file.php#.png
```

</details>

<details>
<summary>Hint 5</summary>

When the target fetches the file, check your HTTP server logs. The fragment should not be included in the request.

</details>

<details>
<summary>Hint 6</summary>

After upload, test whether the PHP file exists directly under the cloud images directory.

</details>

<details>
<summary>Hint 7</summary>

The uploaded files are cleaned up regularly, so be ready to trigger your payload quickly.

</details>

<details>
<summary>Hint 8</summary>

If a normal Bash reverse shell is awkward, try a FIFO plus netcat reverse shell.

</details>

## User Pivot

<details>
<summary>Hint 1</summary>

Once you have a shell as the web user, enumerate readable files owned by other users.

</details>

<details>
<summary>Hint 2</summary>

Look outside the home directories as well. `/opt` is worth checking.

</details>

<details>
<summary>Hint 3</summary>

A KeePass database can be converted into a crackable hash.

</details>

<details>
<summary>Hint 4</summary>

Use `keepass2john` and a common wordlist.

</details>

<details>
<summary>Hint 5</summary>

After cracking the database, inspect its entries for system credentials.

</details>

<details>
<summary>Hint 6</summary>

Use the recovered credentials to become the normal user.

</details>

## Privilege Escalation

<details>
<summary>Hint 1</summary>

Check the normal user’s home directory carefully.

</details>

<details>
<summary>Hint 2</summary>

There is a scripts directory that is not fully owned by the normal user, but part of it is still interesting.

</details>

<details>
<summary>Hint 3</summary>

Read the PHP script and look at what it includes.

</details>

<details>
<summary>Hint 4</summary>

The script explains why uploaded files disappear from the cloud images directory.

</details>

<details>
<summary>Hint 5</summary>

The included PHP file itself may not be writable, but check the permissions of the directory containing it.

</details>

<details>
<summary>Hint 6</summary>

If you can write to a directory, you may be able to rename or replace files inside it even if the original file is owned by root.

</details>

<details>
<summary>Hint 7</summary>

Replace the included PHP library with a function of the same name.

</details>

<details>
<summary>Hint 8</summary>

The replacement function can create a SUID copy of Bash.

</details>

<details>
<summary>Hint 9</summary>

Wait for the scheduled task to run, then check `/tmp`.

</details>

<details>
<summary>Hint 10</summary>

Run the SUID Bash binary with the preserve privileges flag.

</details>

## Final Path

<details>
<summary>Hint 1</summary>

The attack chain is:

```text
Web enumeration
→ URL-based cloud upload
→ URL fragment validation bypass
→ PHP command shell
→ Reverse shell as web user
→ Readable KeePass database
→ Crack KeePass password
→ Recover normal user credentials
→ Abuse root-run PHP include
→ SUID Bash
→ Root
```

</details>
