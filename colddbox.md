# ColddBox

An easy level machine with multiple ways to escalate privileges. By Hixec.

Room: https://tryhackme.com/room/colddboxeasy

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/colddbox.md

## Reconnaissance

<details>
<summary>Hint 1</summary>

Start by identifying all open TCP services. Do not assume SSH is on the default port.

</details>

<details>
<summary>Hint 2</summary>

There are only a couple of exposed services. The web service is the main entry point, but the other service becomes useful later.

</details>

<details>
<summary>Hint 3</summary>

Pay attention to the web stack. This is a WordPress target, so normal WordPress enumeration applies.

</details>

## Web Enumeration

<details>
<summary>Hint 1</summary>

Look for standard WordPress paths such as the login page, content directories, XML-RPC, and readme files.

</details>

<details>
<summary>Hint 2</summary>

A directory brute force should reveal an unusual non-WordPress-looking path. It contains a message rather than a normal application page.

</details>

<details>
<summary>Hint 3</summary>

The unusual path contains a note between named users. It hints that one user changed another user's password.

</details>

<details>
<summary>Hint 4</summary>

The note is not giving you a direct password. It is pointing you toward valid usernames and a likely weak-password situation.

</details>

## WordPress Enumeration

<details>
<summary>Hint 1</summary>

Use WordPress-specific enumeration to find users, themes, plugins, and version information.

</details>

<details>
<summary>Hint 2</summary>

Several WordPress vulnerabilities may appear, but not every vulnerability is the intended route. Focus on practical access.

</details>

<details>
<summary>Hint 3</summary>

The usernames are more useful than the CVE noise. You should end up with multiple candidate usernames.

</details>

<details>
<summary>Hint 4</summary>

The username connected to the room name is especially important.

</details>

## Initial Access

<details>
<summary>Hint 1</summary>

Try a password attack against the WordPress login using the discovered usernames.

</details>

<details>
<summary>Hint 2</summary>

The hidden note suggests a password was changed to something easy enough for another user to continue their work.

</details>

<details>
<summary>Hint 3</summary>

The account you want is not necessarily the person whose password was mentioned in the note.

</details>

<details>
<summary>Hint 4</summary>

Once logged into WordPress, check what privileges the account has. The dashboard can lead to code execution if the account has enough permissions.

</details>

## Getting Code Execution

<details>
<summary>Hint 1</summary>

Look for a place inside WordPress where theme files or plugin files can be edited.

</details>

<details>
<summary>Hint 2</summary>

The active theme is important. Editing a file in an inactive theme may not help.

</details>

<details>
<summary>Hint 3</summary>

If a normal missing page does not load the WordPress 404 template, try accessing the edited theme file more directly.

</details>

<details>
<summary>Hint 4</summary>

A simple server-side command execution test is useful before trying a reverse shell.

</details>

<details>
<summary>Hint 5</summary>

Once code execution works, send yourself a shell and confirm which Linux user you landed as.

</details>

## From Web User to c0ldd

<details>
<summary>Hint 1</summary>

The web shell user is not the final local user. Start by checking the WordPress installation files.

</details>

<details>
<summary>Hint 2</summary>

WordPress configuration files often contain database credentials.

</details>

<details>
<summary>Hint 3</summary>

Do not treat the database password as database-only. Try credential reuse.

</details>

<details>
<summary>Hint 4</summary>

The useful local account matches one of the WordPress users you already found.

</details>

## User Flag

<details>
<summary>Hint 1</summary>

After becoming the local user, check their home directory.

</details>

<details>
<summary>Hint 2</summary>

The user flag is encoded rather than plain text.

</details>

<details>
<summary>Hint 3</summary>

The decoded message is Spanish and indicates the first level is complete.

</details>

## Privilege Escalation

<details>
<summary>Hint 1</summary>

Always check what the local user can run with elevated privileges.

</details>

<details>
<summary>Hint 2</summary>

This user can run several binaries as root. More than one of them can be abused.

</details>

<details>
<summary>Hint 3</summary>

One editor can spawn a shell from inside itself.

</details>

<details>
<summary>Hint 4</summary>

One file-permission tool can be used to change the privilege behavior of an existing shell binary.

</details>

<details>
<summary>Hint 5</summary>

One old network client has an interactive escape feature that can spawn a shell.

</details>

<details>
<summary>Hint 6</summary>

The cleanest writeup route is usually the editor escape, because it avoids permanently changing system binaries.

</details>

## Root Flag

<details>
<summary>Hint 1</summary>

Once root, check the root user's home directory.

</details>

<details>
<summary>Hint 2</summary>

The root flag is also encoded.

</details>

<details>
<summary>Hint 3</summary>

The decoded root message is Spanish and confirms the machine is complete.

</details>

## Nudge Summary

<details>
<summary>Open only if stuck</summary>

The broad route is:

1. Find WordPress on the web service.
2. Enumerate WordPress users.
3. Find the hidden note.
4. Use the note and usernames to guide a WordPress login attack.
5. Log into WordPress as the important user.
6. Use WordPress admin access to get code execution.
7. Read the WordPress config.
8. Reuse credentials to become the matching local user.
9. Check elevated privileges.
10. Abuse one allowed root binary.
11. Decode the user and root flags.

</details>
