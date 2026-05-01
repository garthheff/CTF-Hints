# TryHackMe: Year of the Rabbit Hints

Room: https://tryhackme.com/room/yearoftherabbit  
Theme: Time to enter the warren...

Walthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/yearoftherabbit.md

## Getting Started

<details>
<summary>Hint 1</summary>

Start with broad service enumeration. You are looking for the exposed TCP services and versions before committing to any one path.

</details>

<details>
<summary>Hint 2</summary>

There are only a few open services. The web service is the easiest place to begin because it exposes content you can inspect without credentials.

</details>

<details>
<summary>Hint 3</summary>

When a web page looks simple, do not only browse it visually. Check the page source and linked files.

</details>

## Web Enumeration

<details>
<summary>Hint 1</summary>

Look closely at the stylesheet linked from the main page. Comments in static files can be just as useful as visible page content.

</details>

<details>
<summary>Hint 2</summary>

The clue in the stylesheet points you to another PHP page. Visiting it normally may not show the useful part because it redirects you.

</details>

<details>
<summary>Hint 3</summary>

Use a tool that shows HTTP headers and redirects clearly. The important clue is in the response before the redirect completes.

Example approach:

```bash
curl -v http://TARGET/some-page.php
```

</details>

<details>
<summary>Hint 4</summary>

The redirect leaks a hidden directory path through a query parameter. Try browsing that directory directly.

</details>

## File Inspection

<details>
<summary>Hint 1</summary>

Download the image from the hidden directory. In CTF rooms, image files often contain extra strings, metadata, or embedded content.

</details>

<details>
<summary>Hint 2</summary>

Start simple before using heavier stego tooling. Run `strings` against the image and check near the bottom of the output.

</details>

<details>
<summary>Hint 3</summary>

The image gives you a username and a list of possible passwords. Save the candidate passwords into a wordlist.

</details>

## First Credential Check

<details>
<summary>Hint 1</summary>

You have a username, a password list, and an open FTP service. Test those together.

</details>

<details>
<summary>Hint 2</summary>

Hydra is useful here because the password list is small and targeted.

Example shape:

```bash
hydra -l USERNAME -P passwords.txt ftp://TARGET -V
```

</details>

<details>
<summary>Hint 3</summary>

The FTP credential does not necessarily work over SSH. Use the service it was found for.

</details>

## FTP Enumeration

<details>
<summary>Hint 1</summary>

Log in to FTP and list the available files. There is a credential file worth downloading.

</details>

<details>
<summary>Hint 2</summary>

The downloaded file is not plain text in the usual sense. Look at the symbols and repeated pattern, then identify the encoding language.

</details>

<details>
<summary>Hint 3</summary>

The content is Brainfuck. Decode it to recover the next username and password pair.

</details>

## First Shell

<details>
<summary>Hint 1</summary>

Use the decoded credentials over SSH. This gets you onto the box as a real user.

</details>

<details>
<summary>Hint 2</summary>

Pay attention to the login message. It tells you that another user has a hidden message waiting somewhere on the system.

</details>

<details>
<summary>Hint 3</summary>

Search the filesystem for paths matching the wording used in the login message.

Example shape:

```bash
find / -path '*keyword*' -ls 2>/dev/null
```

</details>

## Moving Laterally

<details>
<summary>Hint 1</summary>

The hidden message is under a games-related path. It is intended for another user.

</details>

<details>
<summary>Hint 2</summary>

Read the hidden file carefully. It contains a password for the next local account.

</details>

<details>
<summary>Hint 3</summary>

Use `su` to switch to the next user, then check their home directory for the user flag.

</details>

## Privilege Escalation Path One

<details>
<summary>Hint 1</summary>

Always check sudo permissions for the current user.

```bash
sudo -l
```

</details>

<details>
<summary>Hint 2</summary>

The rule allows running `vi` against the user flag file, but the target user restriction looks unusual.

</details>

<details>
<summary>Hint 3</summary>

Check the sudo version. Older sudo versions may be vulnerable to a bypass involving numeric user IDs.

</details>

<details>
<summary>Hint 4</summary>

The `!root` restriction can be bypassed on vulnerable sudo versions using a special UID value.

Research clue: sudo Baron Samedit

</details>

<details>
<summary>Hint 5</summary>

Once `vi` opens with elevated privileges, you can escape to a shell from inside `vi`.

Inside `vi`, enter command mode and run:

```vim
:!/bin/bash
```

</details>

<details>
<summary>Hint 6</summary>

After getting a root shell, move to `/root` and read the final flag.

</details>

## Privilege Escalation Path Two

<details>
<summary>Hint 1</summary>

There is another possible route from the first SSH user. Check whether `pkexec` exists and what version is installed.

</details>

<details>
<summary>Hint 2</summary>

The installed `pkexec` version may be exploitable.

</details>

<details>
<summary>Hint 3</summary>

If using an exploit binary, host it from your attack box and download it to the target with `wget`, then mark it executable.

</details>

<details>
<summary>Hint 4</summary>

Run the exploit from the target user shell and verify with `whoami`.

</details>

