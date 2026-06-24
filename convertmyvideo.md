# ConvertMyVideo

My Script to convert videos to MP3 is super secure

Room: https://tryhackme.com/room/convertmyvideo

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/medium/convertmyvideo.md

-----------------------

## Web Enumeration

<details>
<summary>Hint 1</summary>

Start with normal web directory enumeration.

</details>

<details>
<summary>Hint 2</summary>

Use extensions during enumeration. PHP-related paths are relevant.

</details>

<details>
<summary>Hint 3</summary>

Pay attention to paths that return `301` and `401`.

</details>

<details>
<summary>Hint 4</summary>

One discovered path is protected with HTTP Basic Authentication.

</details>

---

## Converter Request

<details>
<summary>Hint 1</summary>

The main page is not just static. Interact with the converter form.

</details>

<details>
<summary>Hint 2</summary>

Intercept the request made when submitting a YouTube URL.

</details>

<details>
<summary>Hint 3</summary>

Look closely at the POST body.

</details>

<details>
<summary>Hint 4</summary>

The parameter containing the submitted YouTube URL is the important one.

</details>

<details>
<summary>Hint 5</summary>

Submit an invalid or incomplete video URL and read the server’s response.

</details>

<details>
<summary>Hint 6</summary>

The response leaks output from the backend tool.

</details>

---

## Initial Foothold

<details>
<summary>Hint 1</summary>

The converter appears to pass user-controlled input to a backend command-line program.

</details>

<details>
<summary>Hint 2</summary>

Test for command injection against the URL parameter.

</details>

<details>
<summary>Hint 3</summary>

A simple separator may not be enough. Try thinking about how shell commands can be separated across lines.

</details>

<details>
<summary>Hint 4</summary>

Use a harmless command first to confirm execution.

</details>

<details>
<summary>Hint 5</summary>

If payloads with spaces fail, consider shell variables that expand to whitespace.

</details>

<details>
<summary>Hint 6</summary>

Before trying to get a shell, identify which interpreters and transfer tools exist on the target.

</details>

<details>
<summary>Hint 7</summary>

A short hosted script is easier than a complex inline reverse shell payload.

</details>

<details>
<summary>Hint 8</summary>

Use the injection to download and execute your script from a temporary location.

</details>

---

## Admin Directory

<details>
<summary>Hint 1</summary>

After getting a shell, return to the protected web path found during enumeration.

</details>

<details>
<summary>Hint 2</summary>

List hidden files, not just normal files.

</details>

<details>
<summary>Hint 3</summary>

One flag is stored directly in the protected directory.

</details>

<details>
<summary>Hint 4</summary>

The authentication files for the protected directory are also present there.

</details>

<details>
<summary>Hint 5</summary>

One hidden file points to the password file used by Basic Auth.

</details>

<details>
<summary>Hint 6</summary>

The Basic Auth hash uses an Apache-specific MD5-style format.

</details>

<details>
<summary>Hint 7</summary>

A standard password-cracking wordlist is enough.

</details>

<details>
<summary>Hint 8</summary>

Obfuscate recovered credentials in public notes.

Example:

```text
username:p*****
```

</details>

---

## Admin Source

<details>
<summary>Hint 1</summary>

Read the PHP source inside the protected directory.

</details>

<details>
<summary>Hint 2</summary>

Look for a PHP function that runs operating system commands.

</details>

<details>
<summary>Hint 3</summary>

This shows the protected area also contains a command execution feature.

</details>

<details>
<summary>Hint 4</summary>

The public converter injection was enough for foothold, but the protected page confirms another intended execution path.

</details>

---

## Privilege Escalation

<details>
<summary>Hint 1</summary>

Basic SUID enumeration is useful, but it is not the main path here.

</details>

<details>
<summary>Hint 2</summary>

Watch processes for scheduled root activity.

</details>

<details>
<summary>Hint 3</summary>

Let the process monitor run long enough to catch minute-based jobs.

</details>

<details>
<summary>Hint 4</summary>

Look for root executing a script from a web-writable location.

</details>

<details>
<summary>Hint 5</summary>

Inspect the script being executed by root and check whether your current user can modify it.

</details>

<details>
<summary>Hint 6</summary>

Replace the script with a payload that creates a privileged copy of a shell.

</details>

<details>
<summary>Hint 7</summary>

Use the copied shell in a way that preserves effective privileges.

</details>

---

## Root Flag

<details>
<summary>Hint 1</summary>

After privilege escalation, check the standard root home location.

</details>

<details>
<summary>Hint 2</summary>

Obfuscate flags in public notes.

Example:

```text
flag{d9b3...e94a}
```

</details>

---

## High-Level Solve Path

<details>
<summary>Progression</summary>

```text
Web enumeration
→ identify protected web area and converter functionality
→ inspect converter POST request
→ observe backend tool leakage
→ test command injection in the URL parameter
→ use newline-based command separation
→ work around spacing issues with shell expansion
→ download and execute a small shell script
→ gain web-user shell
→ inspect protected web directory
→ recover flag and Basic Auth hash
→ crack Basic Auth hash
→ inspect protected PHP source
→ monitor processes for root cron activity
→ find root running a writable cleanup script
→ replace script with privileged shell payload
→ preserve privileges
→ root
```

</details>
