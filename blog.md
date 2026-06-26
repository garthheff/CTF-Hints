# Blog

Billy Joel made a Wordpress blog! 

Billy Joel made a blog on his home computer and has started working on it.  It's going to be so awesome!

Enumerate this box and find the 2 flags that are hiding on it!  Billy has some weird things going on his laptop.  Can you maneuver around and get what you need?  Or will you fall down the rabbit hole...

In order to get the blog to work with , you'll need to add 10.64.129.2 blog. to your /etc/hosts file.

Credit to Sq00ky for the root privesc idea ;)

Room: https://tryhackme.com/room/blog

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/medium/blog.md

-----------------

## Enumeration

<details>
<summary>Hint 1</summary>

Start by identifying the exposed services. There is more than just the website available.

</details>

<details>
<summary>Hint 2</summary>

The web service is running a common CMS. Make sure the hostname resolves correctly before relying on browser or CMS enumeration results.

</details>

<details>
<summary>Hint 3</summary>

The file-sharing service allows access without valid credentials. It contains several media files.

</details>

<details>
<summary>Hint 4</summary>

One of the downloaded media files contains hidden data, but it is not the main path. Treat it as a warning not to get dragged too deep into decoys.

</details>

---

## CMS Access

<details>
<summary>Hint 1</summary>

Enumerate the CMS version and users.

</details>

<details>
<summary>Hint 2</summary>

The CMS version has many known issues, but the useful one requires authentication.

</details>

<details>
<summary>Hint 3</summary>

User enumeration gives you a small list of likely login names. Test these against a common password list.

</details>

<details>
<summary>Hint 4</summary>

One of the CMS users has a weak password. The username begins with `k`.

</details>

---

## Foothold

<details>
<summary>Hint 1</summary>

Once authenticated to the CMS, look for an exploit path involving image handling.

</details>

<details>
<summary>Hint 2</summary>

The relevant weakness is an authenticated code execution issue involving image cropping.

</details>

<details>
<summary>Hint 3</summary>

A heavier staged payload may be unstable here. A simpler PHP reverse shell style payload is more reliable.

</details>

<details>
<summary>Hint 4</summary>

A successful exploit should give you execution as the web server user.

</details>

---

## Local Enumeration

<details>
<summary>Hint 1</summary>

Check the user home directory, but do not trust the first flag-looking file you find.

</details>

<details>
<summary>Hint 2</summary>

There is a document in the user’s home directory. Read it carefully for story clues.

</details>

<details>
<summary>Hint 3</summary>

The document mentions a policy issue involving removable media.

</details>

<details>
<summary>Hint 4</summary>

The normal-looking user flag in the home directory is a decoy.

</details>

---

## Privilege Escalation

<details>
<summary>Hint 1</summary>

Look for unusual SUID binaries.

</details>

<details>
<summary>Hint 2</summary>

Most SUID results are standard system binaries. Focus on anything custom or out of place.

</details>

<details>
<summary>Hint 3</summary>

There is a custom binary under a system administration path.

</details>

<details>
<summary>Hint 4</summary>

Inspecting the binary reveals it checks an environment variable.

</details>

<details>
<summary>Hint 5</summary>

The environment variable name is related to being an administrator.

</details>

<details>
<summary>Hint 6</summary>

If the expected environment variable exists, the binary launches a shell with elevated privileges.

</details>

---

## User Flag

<details>
<summary>Hint 1</summary>

The user flag in the home directory is not the real one.

</details>

<details>
<summary>Hint 2</summary>

The document’s removable media clue points to the real location.

</details>

<details>
<summary>Hint 3</summary>

After privilege escalation, search for all files named `user.txt`.

</details>

<details>
<summary>Hint 4</summary>

The real user flag is stored on mounted removable media.

</details>

---

## Root Flag

<details>
<summary>Hint 1</summary>

The custom SUID binary is the root path.

</details>

<details>
<summary>Hint 2</summary>

The binary checks for an environment variable before launching a shell.

</details>

<details>
<summary>Hint 3</summary>

Set the expected environment variable, then run the binary again.

</details>

<details>
<summary>Hint 4</summary>

Once you have a root shell, the root flag is in the usual root-only location.

</details>

