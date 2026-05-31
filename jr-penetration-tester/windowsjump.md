# Windows Jump

Room: https://tryhackme.com/room/windowsjump

Walthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/jr-penetration-tester/windowsjump.md

Use privilege escalation knowledge to jump from a guest user to SYSTEM.

A routine vulnerability scan flagged a Windows machine on the internal network; nothing alarming on the surface, just a standard workstation left behind after a round of layoffs. IT never cleaned it up properly. Your job is to find out how badly. Your objective is to escalate from guest access all the way through:  

guest->thmuser->notadmin->svcadmin->SYSTEM

# Windows Privilege Escalation Hints

## guest -> thmuser

<details>
<summary>Hint 1</summary>

Start with SMB. Confirm whether guest access works without credentials.

</details>

<details>
<summary>Hint 2</summary>

After confirming guest access, enumerate available SMB shares and focus on anything readable by guest.

</details>

<details>
<summary>Hint 3</summary>

The interesting share is meant to be public. Look for an onboarding or welcome-style file.

</details>

<details>
<summary>Hint 4</summary>

The public file leaks the next user's login details. Use them to move from guest access to the first real user.

</details>

<details>
<summary>Hint 5</summary>

Once logged in as the first user, check their desktop for the first flag.

</details>

---

## thmuser -> notadmin

<details>
<summary>Hint 1</summary>

The first user has limited privileges, so do not expect token-based escalation from this account.

</details>

<details>
<summary>Hint 2</summary>

Run local enumeration and pay attention to stored credentials rather than services at this stage.

</details>

<details>
<summary>Hint 3</summary>

Look for Windows AutoLogon configuration.

</details>

<details>
<summary>Hint 4</summary>

The useful registry area is the Winlogon key under HKLM.

</details>

<details>
<summary>Hint 5</summary>

The registry reveals the next account in the chain. Use that account to access its own desktop and collect the second flag.

</details>

---

## notadmin -> svcadmin

<details>
<summary>Hint 1</summary>

The second user is also low privilege. Start by enumerating services and service accounts.

</details>

<details>
<summary>Hint 2</summary>

Look for a non-standard service that does not run as LocalSystem, LocalService, or NetworkService.

</details>

<details>
<summary>Hint 3</summary>

The important service runs as the next user in the chain.

</details>

<details>
<summary>Hint 4</summary>

Check the service binary path and then inspect file and folder permissions on that path.

</details>

<details>
<summary>Hint 5</summary>

The service executable is writable. Replacing it with a controlled executable lets you execute code as the service account.

</details>

<details>
<summary>Hint 6</summary>

A service start timeout is not necessarily failure. Check your listener before assuming the payload did not run.

</details>

<details>
<summary>Hint 7</summary>

After getting a shell as the service account, check that user's desktop for the third flag.

</details>

---

## svcadmin -> SYSTEM

<details>
<summary>Hint 1</summary>

The service account is still low privilege. Do not rely on Potato-style escalation unless the right token privilege is present.

</details>

<details>
<summary>Hint 2</summary>

Focus on scheduled tasks and writable task-related files.

</details>

<details>
<summary>Hint 3</summary>

The legacy scheduled task folder is important.

</details>

<details>
<summary>Hint 4</summary>

Look inside the legacy task folder for a script owned by a privileged account but modifiable by the service account.

</details>

<details>
<summary>Hint 5</summary>

The target script is a cleanup batch file. It appears harmless, but it is executed automatically by a task.

</details>

<details>
<summary>Hint 6</summary>

Before using a payload, replace the script contents with a harmless proof command that writes the current execution context to a file.

</details>

<details>
<summary>Hint 7</summary>

If the proof output shows SYSTEM, replace the script with either a flag-read command or a reverse shell command.

</details>

<details>
<summary>Hint 8</summary>

You do not need to manually trigger the task. It runs automatically after a short wait.

</details>

<details>
<summary>Hint 9</summary>

Once SYSTEM execution is confirmed, read the final flag from the root of the C drive.

</details>

---

## Final Review Hints

<details>
<summary>Full path summary</summary>

The intended path is:

```text
guest -> thmuser -> notadmin -> svcadmin -> SYSTEM
```

</details>

<details>
<summary>Vulnerability summary</summary>

The chain relies on:

```text
Readable public SMB share
Stored AutoLogon credentials
Writable service binary
Writable scheduled-task batch file
```

</details>

<details>
<summary>Flag locations summary</summary>

The flags are located in each user's desktop path, with the final flag at the root of the C drive.

</details>
