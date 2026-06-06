# U.A. High School

Room: https://tryhackme.com/room/yueiua

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/yueiua.md

Welcome to the web application of U.A., the Superhero Academy.

Join us in the mission to protect the digital world of superheroes! U.A., the most renowned Superhero Academy, is looking for a superhero to test the security of our new site.

Our site is a reflection of our school values, designed by our engineers with incredible Quirks. We have gone to great lengths to create a secure platform that reflects the exceptional education of the U.A.

---

## Enumeration

<details>
<summary>Hint 1</summary>

Start with a full TCP scan. The attack surface is small, so focus on the services that are actually open.

</details>

<details>
<summary>Hint 2</summary>

The web service is the important one at the start. Browse the site and check the linked pages.

</details>

<details>
<summary>Hint 3</summary>

Directory enumeration should find the normal static pages and an `assets` directory.

</details>

<details>
<summary>Hint 4</summary>

Do not spend too long on virtual hosts. The useful path is reachable from the main site.

</details>

---

## Web Foothold

<details>
<summary>Hint 1</summary>

Look closely at the files under the `assets` directory.

</details>

<details>
<summary>Hint 2</summary>

There is a PHP file in the assets path that appears empty at first glance.

</details>

<details>
<summary>Hint 3</summary>

Test whether the PHP file accepts parameters. One common parameter name is useful here.

</details>

<details>
<summary>Hint 4</summary>

The vulnerable endpoint allows command execution through a parameter named `cmd`.

</details>

<details>
<summary>Hint 5</summary>

Use the command execution to call back to your listener with a PHP reverse shell. Make sure the callback IP is your VPN or AttackBox IP.

</details>

---

## From www-data to deku

<details>
<summary>Hint 1</summary>

Once you have a shell, check the web root and nearby directories.

</details>

<details>
<summary>Hint 2</summary>

There is hidden content outside the normal `/var/www/html` path.

</details>

<details>
<summary>Hint 3</summary>

A file in `/var/www/Hidden_Content` contains encoded text.

</details>

<details>
<summary>Hint 4</summary>

Decode the text as base64. The decoded value is not the Linux password directly.

</details>

<details>
<summary>Hint 5</summary>

The decoded value is a passphrase. Look for a file that would need a passphrase.

</details>

---

## Image Puzzle

<details>
<summary>Hint 1</summary>

Check the image files in the web assets directory.

</details>

<details>
<summary>Hint 2</summary>

Do not trust file extensions. Use `file` and `xxd` to inspect the image headers.

</details>

<details>
<summary>Hint 3</summary>

One image has a misleading or corrupted header.

</details>

<details>
<summary>Hint 4</summary>

The corrupted image begins with bytes that look like PNG, but the rest behaves like JPEG data.

</details>

<details>
<summary>Hint 5</summary>

Repair the image header so the file is treated as a JPEG, then retry steganography extraction.

</details>

<details>
<summary>Hint 6</summary>

Use the decoded passphrase from the hidden content file with `steghide`.

</details>

<details>
<summary>Hint 7</summary>

The extracted file contains credentials for the user `deku`. Mask them in notes as `deku:One?...1/A`.

</details>

---

## User Flag

<details>
<summary>Hint 1</summary>

After extracting the credentials, switch to or SSH in as `deku`.

</details>

<details>
<summary>Hint 2</summary>

The user flag is in `deku`'s home directory.

</details>

<details>
<summary>Hint 3</summary>

Mask the user flag in notes as `THM{W3l...All??}`.

</details>

---

## Privilege Escalation

<details>
<summary>Hint 1</summary>

Check what `deku` can run with sudo.

</details>

<details>
<summary>Hint 2</summary>

There is a sudo-allowed script under `/opt/NewComponent`.

</details>

<details>
<summary>Hint 3</summary>

The script cannot simply be overwritten, even though the ownership may look interesting.

</details>

<details>
<summary>Hint 4</summary>

Inspect the script. The dangerous line is the one using `eval` on user-controlled input.

</details>

<details>
<summary>Hint 5</summary>

The script filters many obvious command injection characters, but it does not prevent output redirection.

</details>

<details>
<summary>Hint 6</summary>

Because the script runs through sudo, redirection can be abused to write a root-owned sudoers rule.

</details>

<details>
<summary>Hint 7</summary>

The payload format is a sudoers entry for `deku`, redirected into `/etc/sudoers.d/`.

</details>

<details>
<summary>Hint 8</summary>

After writing the sudoers rule, check `sudo -l` again. You should be able to run a root shell without a password.

</details>

---

## Final Spoiler Path

<details>
<summary>Final spoiler</summary>

The overall chain is:

1. Enumerate HTTP and find `/assets/index.php`.
2. Use the `cmd` parameter for command execution.
3. Catch a reverse shell as `www-data`.
4. Find `/var/www/Hidden_Content/passphrase.txt`.
5. Base64 decode the passphrase, masked as `All...!!!`.
6. Find `oneforall.jpg` in the image assets.
7. Repair the corrupted JPEG header.
8. Use `steghide` with the passphrase to extract credentials for `deku`.
9. Read the user flag, masked as `THM{W3l...All??}`.
10. Run `sudo -l` and inspect `/opt/NewComponent/feedback.sh`.
11. Abuse `eval` plus redirection to create a sudoers rule for `deku`.
12. Run a root shell and read the root flag, masked as `THM{Y0U...H3r0}`.

</details>
