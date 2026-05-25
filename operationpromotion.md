# Operation Promotion

Room: https://tryhackme.com/room/operationpromotion

Walthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/operationpromotion.md

One engagement stands between you and your next title.

you are up for promotion at Hadron Security. Your senior lead, Mara, has handed you a solo engagement against RecruitCorp, a small recruiting firm with a public-facing portal. Compromise the host, capture the flags, and demonstrate that you are ready for the Penetration Tester title.

Start the by clicking the Start Machine button at the top-right of the task. You can complete the challenge by connecting through or the AttackBox, which contains all the essential tools.

Allow two to three minutes for all services to start.

---

## Where is the hidden admin area?

<details>
<summary>Hint 1</summary>

Check common web discovery files before brute forcing everything.

</details>

<details>
<summary>Hint 2</summary>

The site tells search engines not to index a sensitive path.

</details>

<details>
<summary>Hint 3</summary>

Review `robots.txt`.

</details>

---

## How do you access the admin panel?

<details>
<summary>Hint 1</summary>

The admin panel uses a simple username and password form.

</details>

<details>
<summary>Hint 2</summary>

Test the login fields for common input validation mistakes.

</details>

<details>
<summary>Hint 3</summary>

The username field is vulnerable to a classic SQL injection login bypass.

</details>

---

## Where do you find the next internal endpoint?

<details>
<summary>Hint 1</summary>

After entering the admin panel, look for user-related functionality.

</details>

<details>
<summary>Hint 2</summary>

The user lookup endpoint accepts a numeric ID.

</details>

<details>
<summary>Hint 3</summary>

Enumerate user IDs and pay close attention to the notes field.

</details>

<details>
<summary>Hint 4</summary>

One system account note references a maintenance ping endpoint.

</details>

---

## How do you get command execution?

<details>
<summary>Hint 1</summary>

The maintenance endpoint performs a network diagnostic against a user-supplied target.

</details>

<details>
<summary>Hint 2</summary>

If an application runs a shell command using your input, command separators may be useful.

</details>

<details>
<summary>Hint 3</summary>

Test the ping target parameter with a harmless command first.

</details>

<details>
<summary>Hint 4</summary>

Once confirmed, use the injection to get a reverse shell as the web server user.

</details>

---

## Where is the next credential clue?

<details>
<summary>Hint 1</summary>

After getting a shell, inspect the web application files.

</details>

<details>
<summary>Hint 2</summary>

Configuration files are often stored outside the main source logic.

</details>

<details>
<summary>Hint 3</summary>

Look under the web config directory for database settings.

</details>

<details>
<summary>Hint 4</summary>

The config mentions a local Linux user and contains a bcrypt hash.

</details>

---


## How do you crack the local user's password?

<details>
<summary>Hint 1</summary>

The hash in the config is bcrypt, so avoid very large wordlists unless necessary.

</details>

<details>
<summary>Hint 2</summary>

The main website contains words that are likely part of the password theme.

</details>

<details>
<summary>Hint 3</summary>

Use CeWL against the main RecruitCorp website to build a targeted wordlist.

</details>

<details>
<summary>Hint 4</summary>

Pay attention to season and year wording on the homepage.

</details>

<details>
<summary>Hint 5</summary>

Small, targeted mutations of the CeWL list are more practical than a full generic wordlist against bcrypt.

</details>

<details>
<summary>Hint 6</summary>

a year and !

</details>

---

## How do you become the local user?

<details>
<summary>Hint 1</summary>

Once the bcrypt hash is cracked, try the password with the local user named in the config.

</details>

<details>
<summary>Hint 2</summary>

SSH is available and accepts password authentication.

</details>

<details>
<summary>Hint 3</summary>

After login, check the user's home directory for the user flag.

</details>

</details>

---

## How do you escalate to root?

<details>
<summary>Hint 1</summary>

After becoming the local user, check sudo permissions.

</details>

<details>
<summary>Hint 2</summary>

The user can run a common file-searching binary as root without a password.

</details>

<details>
<summary>Hint 3</summary>

That binary supports executing another command.

</details>

<details>
<summary>Hint 4</summary>

Use the allowed sudo command to execute a privileged shell.

</details>

---

## What is the root flag path?

<details>
<summary>Hint 1</summary>

Once you have a root shell, check root's home directory.

</details>

<details>
<summary>Hint 2</summary>

The flag file is not named `root.txt` on this box.

</details>

</details>
