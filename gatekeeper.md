# Gatekeeper

Can you get past the gate and through the fire?

Defeat the Gatekeeper to break the chains.  But beware, fire awaits on the other side.

Room: https://tryhackme.com/room/gatekeeper

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/medium/gatekeeper.md

---

## Initial Enumeration

<details>
<summary>Hint 1</summary>

Start with a full TCP port scan. This is a Windows target, so expect common Windows services, but pay close attention to any unusual high-numbered port.

</details>

<details>
<summary>Hint 2</summary>

One custom service responds with a simple greeting and echoes user-controlled input. Interacting with it manually can reveal how it handles strings.

</details>

<details>
<summary>Hint 3</summary>

SMB exposes a share that is readable without valid credentials. Look through the accessible user/share directories carefully.

</details>

---

## Custom Service

<details>
<summary>Hint 1</summary>

The unusual service accepts text input and returns it inside a formatted response.

</details>

<details>
<summary>Hint 2</summary>

Try sending progressively longer input. A normal response at a small size and a reset/refused connection at a larger size is an important sign.

</details>

<details>
<summary>Hint 3</summary>

Avoid repeatedly crashing the remote service. Look for a way to obtain the service binary and test it locally instead.

</details>

---

## SMB Discovery

<details>
<summary>Hint 1</summary>

The readable SMB share contains more than default Windows profile files.

</details>

<details>
<summary>Hint 2</summary>

Inside a shared folder, there is a Windows executable related to the custom service.

</details>

<details>
<summary>Hint 3</summary>

Download the executable and inspect it locally. Its strings and metadata strongly hint at the intended vulnerability class.

</details>

---

## Binary Analysis

<details>
<summary>Hint 1</summary>

Check whether the binary is 32-bit or 64-bit, and whether common exploit protections are enabled.

</details>

<details>
<summary>Hint 2</summary>

The binary is suitable for a classic stack-based buffer overflow exercise.

</details>

<details>
<summary>Hint 3</summary>

Look for references to the greeting format string and imported networking/string-handling functions.

</details>

---

## Local Crash Testing

<details>
<summary>Hint 1</summary>

A Windows debugger is helpful, but not strictly required. The executable can be run locally under a compatibility layer on Linux.

</details>

<details>
<summary>Hint 2</summary>

Run the service locally, send a cyclic pattern, and observe the crash register output.

</details>

<details>
<summary>Hint 3</summary>

The overwritten instruction pointer can be mapped back into the cyclic pattern to find the exact offset.

</details>

---

## Exploit Control

<details>
<summary>Hint 1</summary>

After finding the offset, confirm control by replacing the instruction pointer with a recognisable marker.

</details>

<details>
<summary>Hint 2</summary>

The stack pointer lands near attacker-controlled data after the crash.

</details>

<details>
<summary>Hint 3</summary>

Search the main executable for a reliable instruction that redirects execution to the stack.

</details>

---

## Payload Building

<details>
<summary>Hint 1</summary>

The final payload layout is:

```text
padding
return address
small landing area
reverse shell payload
```

</details>

<details>
<summary>Hint 2</summary>

Exclude null bytes and line-ending characters when generating the payload.

</details>

<details>
<summary>Hint 3</summary>

Use your VPN/AttackBox tunnel IP as the callback address, not localhost.

</details>

<details>
<summary>Hint 4</summary>

Start your listener before sending the final payload to the remote service.

</details>

---

## User Access

<details>
<summary>Hint 1</summary>

Once the exploit works, the shell lands as a regular desktop user.

</details>

<details>
<summary>Hint 2</summary>

Check the current directory first. The user flag is nearby.

</details>

<details>
<summary>Hint 3</summary>

Use Windows-native commands in the shell. Linux commands will not work inside `cmd.exe`.

</details>

---

## Privilege Escalation

<details>
<summary>Hint 1</summary>

List local users and local administrators. One non-default user is a member of the administrators group.

</details>

<details>
<summary>Hint 2</summary>

A desktop shortcut points toward the intended privilege escalation path.

</details>

<details>
<summary>Hint 3</summary>

Look inside the compromised user’s browser profile directory.

</details>

---

## Browser Credential Extraction

<details>
<summary>Hint 1</summary>

The useful browser files are stored under the user’s roaming application data.

</details>

<details>
<summary>Hint 2</summary>

The important files include the saved-login database and the key database.

</details>

<details>
<summary>Hint 3</summary>

Instead of transferring files over a fragile reverse shell, copy the browser credential files into a readable SMB share and pull them from your AttackBox.

</details>

<details>
<summary>Hint 4</summary>

Use a Firefox credential decryption tool locally against the copied profile files.

</details>

<details>
<summary>Hint 5</summary>

If the decryption tool complains about the profile directory, point it directly at the profile folder that contains the login and key database files.

</details>

---

## Administrator Access

<details>
<summary>Hint 1</summary>

The recovered browser credential belongs to the administrator-group user found earlier.

</details>

<details>
<summary>Hint 2</summary>

WinRM is not the expected route unless its port is open.

</details>

<details>
<summary>Hint 3</summary>

Since the recovered user is a local administrator, try authenticating to the administrative SMB share.

</details>

---

## Root Flag

<details>
<summary>Hint 1</summary>

After authenticating as the administrator-group user, browse their desktop through the administrative share.

</details>

<details>
<summary>Hint 2</summary>

In `smbclient`, use file transfer commands rather than shell commands like `cat` or `type`.

</details>

<details>
<summary>Hint 3</summary>

Download the flag file locally, then read it from your AttackBox.

</details>

---

## Key Takeaways

<details>
<summary>Hint 1</summary>

The initial foothold is a classic Windows stack buffer overflow against a custom TCP service.

</details>

<details>
<summary>Hint 2</summary>

Local debugging can be done without a Windows VM by running the vulnerable binary locally and reading the crash output.

</details>

<details>
<summary>Hint 3</summary>

The privilege escalation is credential reuse from saved Firefox logins, not a Windows kernel exploit.

</details>

<details>
<summary>Hint 4</summary>

When a reverse shell is unstable, use existing shares or file-copy side channels instead of forcing interactive transfer methods.

</details>
