# Operation Coldstart

Room: https://tryhackme.com/room/operationcoldstart

Walkthroughs: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/operationcoldstart.md

Wake up the staging server everyone left behind.

Volt Labs, a small shop, suspects an old staging server has rotted into an exposed liability. Mara has assigned you the engagement. Find your way in and demonstrate full compromise.

Start the by clicking the Start Machine button at the top right of the task. You can complete the challenge by connecting through or the AttackBox, which contains all the essential tools.

Allow two to three minutes for all services to start.

-------

## Service Enumeration

<details>
<summary>Hint 1</summary>

Start by identifying the exposed services and versions. There are only a few open ports, so each one is likely important.

</details>

<details>
<summary>Hint 2</summary>

The web service is running behind Gunicorn and presents itself as a URL preview tool.

</details>

<details>
<summary>Hint 3</summary>

Do not ignore FTP. A simple configuration issue may expose files without valid user credentials.

</details>

---

## FTP Access

<details>
<summary>Hint 1</summary>

Try logging in with a common unauthenticated FTP account.

</details>

<details>
<summary>Hint 2</summary>

Once logged in, enumerate directories carefully rather than only checking the FTP root.

</details>

<details>
<summary>Hint 3</summary>

Look for archived project files or backups. Small backup archives can still contain very useful source code.

</details>

---

## Backup Review

<details>
<summary>Hint 1</summary>

Extract the archive and review the project structure.

</details>

<details>
<summary>Hint 2</summary>

The dependency file reveals the technology stack used by the web application.

</details>

<details>
<summary>Hint 3</summary>

The main application source file explains how the URL preview feature works.

</details>

---

## Web Application Logic

<details>
<summary>Hint 1</summary>

Focus on the route that handles the submitted URL.

</details>

<details>
<summary>Hint 2</summary>

The application performs a hostname allow-list check before making the server-side request.

</details>

<details>
<summary>Hint 3</summary>

Look closely at what is checked and what is not checked. The path is not restricted in the same way the hostname is.

</details>

<details>
<summary>Hint 4</summary>

A source code comment reveals an internal hostname that resolves to localhost on the target.

</details>

---

## Internal Admin Access

<details>
<summary>Hint 1</summary>

There is an admin route that should not be accessible from your browser directly.

</details>

<details>
<summary>Hint 2</summary>

The admin route trusts requests coming from localhost.

</details>

<details>
<summary>Hint 3</summary>

Use the URL preview feature to make the server request its own internal admin endpoint.

</details>

<details>
<summary>Hint 4</summary>

Do not use `localhost` as the submitted hostname. Use the internal hostname that the application explicitly allows.

</details>

---

## Getting a Shell

<details>
<summary>Hint 1</summary>

The internal notes disclose staging access details.

</details>

<details>
<summary>Hint 2</summary>

Use the disclosed account against the remote access service found during enumeration.

</details>

<details>
<summary>Hint 3</summary>

After logging in, check the user home directory for the first flag.

</details>

---

## Privilege Escalation Enumeration

<details>
<summary>Hint 1</summary>

After getting a shell, check writable files and directories.

</details>

<details>
<summary>Hint 2</summary>

A writable file under `/opt` points toward a backup-related privilege escalation path.

</details>

<details>
<summary>Hint 3</summary>

Search system configuration for references to the backup directory.

</details>

<details>
<summary>Hint 4</summary>

Cron is involved. Look for a scheduled job that runs as root.

</details>

---

## Backup Cron Job

<details>
<summary>Hint 1</summary>

The root cron job changes into a writable backup directory before creating an archive.

</details>

<details>
<summary>Hint 2</summary>

The dangerous part is not just that root runs the command. It is how the command selects files.

</details>

<details>
<summary>Hint 3</summary>

The wildcard expands filenames before the command runs.

</details>

<details>
<summary>Hint 4</summary>

If you can control filenames in the working directory, you may be able to make filenames that are interpreted as options.

</details>

---

## Tar Wildcard Injection

<details>
<summary>Hint 1</summary>

GNU tar has options that can run an action during archive creation.

</details>

<details>
<summary>Hint 2</summary>

Look into tar checkpoint options.

</details>

<details>
<summary>Hint 3</summary>

Create filenames that tar will interpret as checkpoint-related options.

</details>

<details>
<summary>Hint 4</summary>

The executed action should run a script you control in the writable backup directory.

</details>

---

## Root Access

<details>
<summary>Hint 1</summary>

Your payload does not need to be complex. It only needs to create a reliable way to become root.

</details>

<details>
<summary>Hint 2</summary>

A common CTF approach is to create a root-owned SUID copy of a shell.

</details>

<details>
<summary>Hint 3</summary>

When using a SUID copy of bash, remember to preserve privileges when starting it.

</details>

<details>
<summary>Hint 4</summary>

Once root, check the root home directory for the final flag.

</details>

---

## Root Cause Reflection

<details>
<summary>Hint 1</summary>

The first major issue is unnecessary exposure of application source through anonymous FTP.

</details>

<details>
<summary>Hint 2</summary>

The second major issue is trusting a hostname allow-list while still allowing server-side requests to internal-only paths.

</details>

<details>
<summary>Hint 3</summary>

The final issue is a root scheduled task processing user-writable content with unsafe wildcard expansion.

</details>

<details>
<summary>Hint 4</summary>

A safer backup design would avoid processing attacker-controlled filenames as command arguments and would not let low-privilege users write into a root backup working directory.

</details>
