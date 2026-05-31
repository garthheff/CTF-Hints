# Domino

Room: https://tryhackme.com/room/domino

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/jr-penetration-tester/domino.md

from pathlib import Path

## What is the flag found in the admin user's profile notes?

<details>
<summary>Hint 1</summary>

Start by looking for public information that reveals valid employee usernames.

</details>

<details>
<summary>Hint 2</summary>

The login form gives away the expected username format.

</details>

<details>
<summary>Hint 3</summary>

Once you have a normal user session, look closely at any profile API link shown in the portal.

</details>

<details>
<summary>Hint 4</summary>

Ask yourself whether the profile identifier is tied to the logged-in user, or whether it can be changed.

</details>

<details>
<summary>Hint 5</summary>

The admin user's profile contains the flag in the notes field.

</details>

## What is the flag displayed on the admin panel after gaining admin access?

<details>
<summary>Hint 1</summary>

The support ticket feature is more important than it first appears.

</details>

<details>
<summary>Hint 2</summary>

Test whether ticket content is rendered or processed in a way that can make the reviewer’s browser or bot contact you.

</details>

<details>
<summary>Hint 3</summary>

The admin reviewer carries an authenticated session.

</details>

<details>
<summary>Hint 4</summary>

You do not need the admin password if you can capture and reuse the admin session value. (XSS)

</details>

<details>
<summary>Hint 5</summary>

After becoming the admin user, check the admin panel for the flag.

</details>

## What is the flag obtained after achieving remote code execution on the server? Flag is stored in /opt/flag3.txt

<details>
<summary>Hint 1</summary>

The backup directory points you toward an encrypted configuration file and a client-side key reference.

</details>

<details>
<summary>Hint 2</summary>

The JavaScript contains the AES key clue, but the key length needs to be corrected for AES-128.

</details>

<details>
<summary>Hint 3</summary>

The decrypted configuration confirms the production environment and points toward the devops context.

</details>

<details>
<summary>Hint 4</summary>

The file API needs a JWT, but the normal token endpoint does not provide the role needed for privileged file access.

</details>

<details>
<summary>Hint 5</summary>

The leaked key can be reused to sign a stronger JWT.

</details>

<details>
<summary>Hint 6</summary>

Reading the file API source reveals that the escape from the web directory is not path traversal.

</details>

<details>
<summary>Hint 7</summary>

Focus on the remote file handling branch and what it does with fetched PHP content.

</details>

<details>
<summary>Hint 8</summary>

Use that behaviour to execute code on the server, then read the flag file in /opt.

</details>

## What is the flag found in the devops user's home directory?

<details>
<summary>Hint 1</summary>

After gaining code execution as the web user, inspect the web root for configuration for credentails.

</details>

<details>
<summary>Hint 2</summary>

Once you become devops, check the home directory for the flag.

</details>

## What is the root flag?

<details>
<summary>Hint 1</summary>

From the devops account, enumerate writable files outside the web root.

</details>

<details>
<summary>Hint 2</summary>

Pay special attention to scripts owned by root but writable by the devops group.

</details>

<details>
<summary>Hint 3</summary>

A monitoring script is writable by devops.

</details>

<details>
<summary>Hint 4</summary>

Confirm whether that script is run automatically, and by which user.

</details>

<details>
<summary>Hint 5</summary>

Process monitoring shows the monitoring script being executed by root on a schedule.

</details>

<details>
<summary>Hint 6</summary>

Place a root shell payload in the script and wait for the scheduled task to run.

</details>

<details>
<summary>Hint 7</summary>

Once you have a root shell, check root’s home directory for the final flag.

</details>



