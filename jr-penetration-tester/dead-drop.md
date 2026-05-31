# Dead Drop

Room: https://tryhackme.com/room/dead-drop

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/jr-penetration-tester/dead-drop.md

Every dead drop points inward. Chain your findings, pivot through the gaps, and follow the trail until nothing is out of reach.

You have been engaged as a penetration tester for a security audit of DeadDrop Ltd, a document management company that provides file-sharing services to corporate clients. The company recently expanded its infrastructure and wants assurance that its systems are secure before onboarding a major new client.

Your point of entry is a web-facing file-sharing application. Behind it sits an internal corporate network that you have no direct access to. Your objective is clear: compromise the domain controller and retrieve the flag from the Administrator's desktop. How you get there is up to you.

------------------

## What password grants you SSH access to the web server?

<details>
<summary>Hint 1</summary>

The login page does not need valid credentials if you can change the logic of the authentication query.

</details>

<details>
<summary>Hint 2</summary>

After logging in, focus on the upload and preview workflow. The useful file type is not PHP.

</details>

<details>
<summary>Hint 3</summary>

The preview feature handles JavaScript differently from other file types. A valid server-side JavaScript module can reveal what user the application is running as.

</details>

<details>
<summary>Hint 4</summary>

If your reverse shell locks the web application, the command is probably blocking. Use a non-blocking approach for callbacks.

</details>

<details>
<summary>Hint 5</summary>

Check the application directory carefully after getting a shell. A backup file contains material that can be cracked offline.

</details>

<details>
<summary>Hint 6</summary>

The cracked account has a normal shell and can be used for SSH access.

</details>

## What credentials does the company's internal mobile application contain? Format: username:password

<details>
<summary>Hint 1</summary>

After logging in with the SSH-capable account, check the user's home directory for backup material.

</details>

<details>
<summary>Hint 2</summary>

The interesting backup is an Android application package.

</details>

<details>
<summary>Hint 3</summary>

Start simple. Search the APK for readable strings before fully decompiling it.

</details>

<details>
<summary>Hint 4</summary>

Once you find an interesting password-like string, identify which part of the APK contains it.

</details>

<details>
<summary>Hint 5</summary>

If the string appears in Android resources, decode the APK and inspect the resource XML files.

</details>

<details>
<summary>Hint 6</summary>

The username and password are stored near each other as default application values.

</details>

## What Active Directory permission does your domain account hold that can be abused for privilege escalation?

<details>
<summary>Hint 1</summary>

The mobile application credentials are not local Linux credentials. Try them against the internal Windows domain.

</details>

<details>
<summary>Hint 2</summary>

The AttackBox cannot directly reach every internal host. The compromised web server can.

</details>

<details>
<summary>Hint 3</summary>

Use a pivot through the web server so your AD tooling can reach the Domain Controller.

</details>

<details>
<summary>Hint 4</summary>

Once domain authentication works, enumerate writable or controllable AD objects.

</details>

<details>
<summary>Hint 5</summary>

The important permission is related to adding an account into a group.

</details>

## What is the name of the group you target to escalate to Domain Admin?

<details>
<summary>Hint 1</summary>

Do not only look at default Active Directory groups.

</details>

<details>
<summary>Hint 2</summary>

Review the writable AD objects and look for a custom group with an admin or support theme.

</details>

<details>
<summary>Hint 3</summary>

The group name is 16 characters long.

</details>

<details>
<summary>Hint 4</summary>

The target group appears as a common name under the domain's Users container.

</details>

## What is the flag on the Domain Controller?

<details>
<summary>Hint 1</summary>

After identifying the group abuse path, add the domain user to the target group.

</details>

<details>
<summary>Hint 2</summary>

Verify that the new privileges allow remote command execution against the Domain Controller.

</details>

<details>
<summary>Hint 3</summary>

The flag is in a very common Windows CTF location.

</details>

<details>
<summary>Hint 4</summary>

List the Administrator desktop on the Domain Controller.

</details>

<details>
<summary>Hint 5</summary>

Read the text file from the Administrator desktop.

</details>

## General Hints

<details>
<summary>Hint 1</summary>

The upload bug is not PHP execution. Think about what the preview feature is doing with JavaScript files.

</details>

<details>
<summary>Hint 2</summary>

When a callback fails, check the VPN interface IP you are using. The target must be able to route to that address.

</details>

<details>
<summary>Hint 3</summary>

Use TCP scans through the pivot. ICMP may fail even when the pivot is working.

</details>

<details>
<summary>Hint 4</summary>

For Active Directory questions, the expected answer may be the BloodHound-style permission name rather than the wording shown by every tool.

</details>
