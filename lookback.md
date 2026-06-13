# Lookback

You’ve been asked to run a vulnerability test on a production environment.

The Lookback company has just started the integration with Active Directory. Due to the coming deadline, the system integrator had to rush the deployment of the environment. Can you spot any vulnerabilities?

Start the Lab Machine by pressing the Start Lab Machine button at the top of this task. You may access the using the AttackBox or your connection. This machine does not respond to ping (ICMP).

Can you find all the flags?
The takes about 5/10 minutes to fully boot up.

Sometimes to move forward, we have to go backward.
So if you get stuck, try to look back!

Room: https://tryhackme.com/room/lookback

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/lookback.md

----------

## Enumeration

<details>
<summary>Hint 1</summary>

Run a full TCP port scan and pay attention to the web services.

</details>

<details>
<summary>Hint 2</summary>

Inspect the HTTPS certificate for a hostname and domain.

</details>

<details>
<summary>Hint 3</summary>

Add the discovered hostname to `/etc/hosts`.

</details>

---

## First Flag

<details>
<summary>Hint 1</summary>

There is a hidden web directory that is accessible over HTTPS but not HTTP.

</details>

<details>
<summary>Hint 2</summary>

Try directory enumeration and investigate `/test/`.

</details>

<details>
<summary>Hint 3</summary>

The page uses HTTP Basic authentication.

</details>

<details>
<summary>Hint 4</summary>

Try very common default credentials.

</details>

<details>
<summary>Hint 5</summary>

Inspect the authenticated page source carefully.

</details>

---

## Command Execution

<details>
<summary>Hint 1</summary>

The `/test/` page contains a log analyser that accepts a path.

</details>

<details>
<summary>Hint 2</summary>

Consider how the supplied path may be inserted into a PowerShell command.

</details>

<details>
<summary>Hint 3</summary>

Try closing the existing PowerShell expression and appending a harmless command such as `whoami`.

</details>

<details>
<summary>Hint 4</summary>

The comment character can prevent the remainder of the original command from being processed.

</details>

---

## User Flag

<details>
<summary>Hint 1</summary>

Enumerate the directories under `C:\Users`.

</details>

<details>
<summary>Hint 2</summary>

Check the Desktop belonging to the development user.

</details>

<details>
<summary>Hint 3</summary>

Read every `.txt` file found on that Desktop.

</details>

---

## Privilege Escalation

<details>
<summary>Hint 1</summary>

One of the notes mentions an important unfinished security task.

</details>

<details>
<summary>Hint 2</summary>

The server is running Microsoft Exchange.

</details>

<details>
<summary>Hint 3</summary>

Determine the Exchange build from the versioned OWA resource path.

</details>

<details>
<summary>Hint 4</summary>

Search Metasploit for Exchange vulnerabilities involving the word `proxy`.

</details>

<details>
<summary>Hint 5</summary>

Use the module's built-in `check` command before exploitation.

</details>

<details>
<summary>Hint 6</summary>

Automatic mailbox enumeration may fail.

The module allows you to manually provide a known email address.

</details>

<details>
<summary>Hint 7</summary>

Several valid internal email addresses were disclosed in the development user's note.

</details>

<details>
<summary>Hint 8</summary>

The relevant Metasploit option is named:

```text
EMAIL
```

</details>

---

## Administrator Flag

<details>
<summary>Hint 1</summary>

A successful Exchange exploit should provide a privileged session.

</details>

<details>
<summary>Hint 2</summary>

Use `whoami` to verify the resulting privilege level.

</details>

<details>
<summary>Hint 3</summary>

Search the Administrator profile for text files.

</details>

<details>
<summary>Hint 4</summary>

The final flag is under the Administrator user's Documents directory.

</details>

