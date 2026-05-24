# Silent Monitor

Room: https://tryhackme.com/room/silent-monitor

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/silent-monitor.md

Enumerate a running internal service, exploit a vulnerable web application, pivot through the system, and crack your way to root.

Green Lights, Dark Corners

CorpNet's internal network operations centre has been running quietly for years. Monitoring hosts, logging events, and keeping the infrastructure alive. Or so it seems. A tip from a disgruntled contractor suggests that someone on the NOC team has been cutting corners, leaving doors open, and hiding things in places no one thinks to look.

The portal is up. The services show green. The audit log looks clean.

But clean logs can be written by anyone.

Your job is to get in, move through the system, and find out what is really running behind the secret dashboard.

-----------

## Initial Enumeration

<details>
<summary>Hint 1</summary>

Focus on the web service rather than SSH. SSH becomes useful later after credentials are found.

</details>

<details>
<summary>Hint 2</summary>

The non-standard HTTP port is running a Python/Werkzeug application.

</details>

<details>
<summary>Hint 3</summary>

Look for an internal-facing area of the site. The login form is more important than the public landing page.

</details>

---

## Gaining Access to the Internal Portal

<details>
<summary>Hint 1</summary>

Inspect the login form carefully. Note the endpoint it posts to and the names of the submitted fields.

</details>

<details>
<summary>Hint 2</summary>

The login logic does not handle user input safely.

</details>

<details>
<summary>Hint 3</summary>

A successful login redirects to a dashboard. That redirect can be used as a success condition while testing the login.

</details>

<details>
<summary>Hint 4</summary>

The database behind the login is SQLite.

</details>

---

## Finding the Next Vulnerability

<details>
<summary>Hint 1</summary>

After accessing the dashboard, read the audit log entries carefully.

</details>

<details>
<summary>Hint 2</summary>

Some log entries are real and some are deliberately planted background noise. Focus on entries that look like someone was testing input handling.

</details>

<details>
<summary>Hint 3</summary>

The health check feature accepts a target and passes it to a system utility.

</details>

<details>
<summary>Hint 4</summary>

Spaces alone will not give command execution. They may only become extra arguments to the health check command.

</details>

<details>
<summary>Hint 5</summary>

The important character is a newline. URL encoding can help deliver it through the form.

</details>

---

## Getting a Shell

<details>
<summary>Hint 1</summary>

First prove command execution with harmless identity or directory checks.

</details>

<details>
<summary>Hint 2</summary>

The vulnerable health check blocks some common shell separators, but it does not block the separator hinted by the audit log.

</details>

<details>
<summary>Hint 3</summary>

If a direct reverse shell is awkward because of quoting, stage a small shell script from your machine and execute it on the target.

</details>

<details>
<summary>Hint 4</summary>

Use the interface/IP that the target can reach for downloading the script, and the VPN/tun interface for the reverse connection if needed.

</details>

---

## Moving from the Web User to a Real User

<details>
<summary>Hint 1</summary>

Once on the host, inspect the web application directory.

</details>

<details>
<summary>Hint 2</summary>

The application database confirms earlier findings, but the more useful file is the application configuration file.

</details>

<details>
<summary>Hint 3</summary>

A credential in the configuration is labelled for a backup agent. It is not necessarily valid through every local authentication method.

</details>

<details>
<summary>Hint 4</summary>

Try remote login as the account named in the backup-agent section.

</details>

---

## User Flag

<details>
<summary>Hint 1</summary>

After logging in as the backup-agent user, check that user's home directory.

</details>

<details>
<summary>Hint 2</summary>

The user flag is in a standard location for that account.

</details>

---

## Finding the Root Path

<details>
<summary>Hint 1</summary>

Look inside the backup-related directory in the user account.

</details>

<details>
<summary>Hint 2</summary>

The interesting backup is not a compressed archive. It is a credential database.

</details>

<details>
<summary>Hint 3</summary>

The database format is newer than some older cracking helpers support.

</details>

<details>
<summary>Hint 4</summary>

If old tools fail on the database version, use a newer KeePass-to-John converter or a newer John Jumbo build.

</details>

<details>
<summary>Hint 5</summary>

The database password is simple enough to be found with a common wordlist.

</details>

---

## Root Flag

<details>
<summary>Hint 1</summary>

Open the credential database after cracking its password.

</details>

<details>
<summary>Hint 2</summary>

There is an entry clearly labelled for the root account.

</details>

<details>
<summary>Hint 3</summary>

Use the recovered root credential locally from the user shell.

</details>

<details>
<summary>Hint 4</summary>

The root flag is in root's home directory.

</details>
