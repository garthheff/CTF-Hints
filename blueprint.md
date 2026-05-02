# Blueprint

Hack into this Windows machine and escalate your privileges to Administrator.

Room: https://tryhackme.com/room/blueprint

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/blueprint.md

## Enumeration

<details>
<summary>Hint 1</summary>

Start with a basic port scan and identify which web services are exposed.

</details>

<details>
<summary>Hint 2</summary>

There is more than one HTTP service. Check both the standard web ports and the alternate web port.

</details>

<details>
<summary>Hint 3</summary>

osCommerce installation.

</details>

<details>
<summary>Hint 4</summary>

Directory listings are useful here. Do not only check the main page.

</details>

<details>
<summary>Hint 5</summary>

Look inside the osCommerce folders for documentation, installation files, and anything that looks out of place.

</details>

## Web Application

<details>
<summary>Hint 1</summary>

The target is running osCommerce 2.3.4.

</details>

<details>
<summary>Hint 2</summary>

Search for public exploits related to this exact osCommerce version.

</details>

<details>
<summary>Hint 3</summary>

Exploit-DB has a relevant exploit for this application.

</details>

<details>
<summary>Hint 4</summary>

The exploit relies on the installer directory still being present.

</details>

<details>
<summary>Hint 5</summary>

The installer can be abused to write PHP into a configuration file.

</details>

## Exploit Fixes

<details>
<summary>Hint 1</summary>

The public exploit may need small changes before it works cleanly in this lab.

</details>

<details>
<summary>Hint 2</summary>

If HTTPS certificate errors occur, update the Python requests call to ignore certificate validation.

</details>

<details>
<summary>Hint 3</summary>

If the certificate key is considered too weak, you may need to lower the OpenSSL security level in the Python request session.

</details>

<details>
<summary>Hint 4</summary>

If one PHP command execution function is disabled, try another one.

</details>

<details>
<summary>Hint 5</summary>

A working command execution function can be used to create a simple reusable web command runner.

</details>

## Code Execution

<details>
<summary>Hint 1</summary>

Confirm code execution with a harmless command first.

</details>

<details>
<summary>Hint 2</summary>

This is a Windows target, so use Windows commands.

</details>

<details>
<summary>Hint 3</summary>

Good first checks include the current user, hostname, and user directories.

</details>

<details>
<summary>Hint 4</summary>

If the web server runs as a highly privileged account, you may be able to access administrator-owned files.

</details>

<details>
<summary>Hint 5</summary>

Wrap command output in preformatted HTML if the browser output is hard to read.

</details>

## Shell Access

<details>
<summary>Hint 1</summary>

A web command runner is enough to execute commands, but a shell is more comfortable.

</details>

<details>
<summary>Hint 2</summary>

Test outbound connectivity from the target back to your AttackBox before attempting a reverse shell.

</details>

<details>
<summary>Hint 3</summary>

Serving files from your AttackBox with a Python HTTP server is useful here.

</details>

<details>
<summary>Hint 4</summary>

Windows can often download files using certutil or PowerShell.

</details>

<details>
<summary>Hint 5</summary>

Netcat for Windows can be copied to the target and used for a reverse shell or file transfer.

</details>

## Hashes

<details>
<summary>Hint 1</summary>

The target contains a local user named Lab.

</details>

<details>
<summary>Hint 2</summary>

The goal is to obtain the local NTLM hash for the Lab user.

</details>

<details>
<summary>Hint 3</summary>

If you have SYSTEM-level access, Mimikatz can dump local SAM hashes.

</details>

<details>
<summary>Hint 4</summary>

Check the target architecture before choosing the Mimikatz binary.

</details>

<details>
<summary>Hint 5</summary>

If the target is 32-bit, use the 32-bit Mimikatz build.

</details>

<details>
<summary>Hint 6</summary>

Run Mimikatz with privilege debug, token elevation, and SAM dumping.

</details>

<details>
<summary>Hint 7</summary>

The Lab user hash can be cracked offline with a cracking tool or online hash cracking service.

</details>

## Alternate Shortcut

<details>
<summary>Hint 1</summary>

There is a faster route than uploading Mimikatz.

</details>

<details>
<summary>Hint 2</summary>

Check the exposed docs directory on the alternate web service.

</details>

<details>
<summary>Hint 3</summary>

Files named SAM and SYSTEM are not normal osCommerce documentation files.

</details>

<details>
<summary>Hint 4</summary>

If both SAM and SYSTEM are exposed, they can be downloaded directly.

</details>

<details>
<summary>Hint 5</summary>

Use Impacket secretsdump against the downloaded hive files to extract local NTLM hashes.

</details>

## Final Access

<details>
<summary>Hint 1</summary>

Once you have command execution as SYSTEM, check the Administrator profile.

</details>

<details>
<summary>Hint 2</summary>

The final file is on the Administrator desktop.

</details>

<details>
<summary>Hint 3</summary>

Be careful with the filename. It has a double extension.

</details>

</details>

