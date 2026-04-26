# Wgel CTF Hints

Room: https://tryhackme.com/room/wgelctf

Have fun with this easy box.

These hints are designed to be progressive. Open them one at a time if you want to avoid spoilers.

---

# User

## Emulation hint

<details>
<summary>Hint 1</summary>

Start with normal service enumeration. Do not assume the web page is the only thing worth checking.

</details>

<details>
<summary>Hint 2</summary>

Once you find the web service, inspect more than just what is rendered in the browser.

</details>

<details>
<summary>Hint 3</summary>

Page source can contain comments that do not appear visually on the page.

</details>

<details>
<summary>Hint 4</summary>

One comment gives you a useful human clue. Do not overthink it.

</details>

---

## Web enumeration hint

<details>
<summary>Hint 1</summary>

Use directory brute forcing against the web root.

</details>

<details>
<summary>Hint 2</summary>

A normal medium sized directory wordlist is enough to discover an interesting directory.

</details>

<details>
<summary>Hint 3</summary>

When you discover a new directory, enumerate that directory as well.

</details>

<details>
<summary>Hint 4</summary>

Hidden dot directories can be exposed by poor web server configuration.

</details>

<details>
<summary>Hint 5</summary>

Look for a directory that would normally belong inside a Linux user home directory, not inside a public web path.

</details>

---

## Access hint

<details>
<summary>Hint 1</summary>

The exposed directory contains something that can be used with SSH.

</details>

<details>
<summary>Hint 2</summary>

You should have already seen the username earlier during web enumeration.

</details>

<details>
<summary>Hint 3</summary>

Use the discovered key with the discovered username to log in over SSH.

</details>

---

## User flag hint

<details>
<summary>Hint 1</summary>

After logging in, start with the user's home directory.

</details>

<details>
<summary>Hint 2</summary>

Check common user folders rather than system directories first.

</details>

<details>
<summary>Hint 3</summary>

The user flag is in one of the visible folders in the user's home directory.

</details>

</details>

---

# Admin

## Escalation hint

<details>
<summary>Hint 1</summary>

Always check what the current user can run with elevated permissions.

</details>

<details>
<summary>Hint 2</summary>

The interesting sudo rule allows running a common download tool as root.

</details>

<details>
<summary>Hint 3</summary>

Search GTFOBins for the allowed binary.

</details>

<details>
<summary>Hint 4</summary>

Not every GTFOBins method works on every version. If one option is unsupported, try another method for the same binary.

</details>

<details>
<summary>Hint 5</summary>

You do not need a full root shell to read the admin flag. You can make the allowed binary read a root-owned file and send it to your machine.

</details>

---

## Exfiltration hint

<details>
<summary>Hint 1</summary>

Start a listener on your attack machine.

</details>

<details>
<summary>Hint 2</summary>

The allowed binary can send local file contents in an HTTP request.

</details>

<details>
<summary>Hint 3</summary>

Make sure the target connects back to your VPN or tunnel IP, not localhost.

</details>

<details>
<summary>Hint 4</summary>

The root flag path follows the same naming style as the user flag.

</details>

<details>
<summary>Hint 5</summary>

Use the root permission on the allowed binary to read the root-only file, then POST it back to your listener.

</details>

---

## Optional unattended root

<details>
<summary>Hint 1</summary>

Because the machine is old, it is worth checking common SUID binaries and CVE's since room was made

</details>

<details>
<summary>Hint 2</summary>

Check the version of the policy authorization helper.

</details>

<details>
<summary>Hint 3</summary>

If you test a public exploit, first check the target architecture.

</details>

<details>
<summary>Hint 4</summary>

A binary compiled for the wrong architecture will not execute on the target.

</details>

<details>
<summary>Hint 5</summary>

This target is 32-bit, so any compiled helper must match that architecture.

</details>

</details>


