# Chocolate Factory

Room: https://tryhackme.com/room/chocolatefactory

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/chocolatefactory.md

A Charlie And The Chocolate Factory themed room, revisit Willy Wonka's chocolate factory!

## Enumeration

<details>
<summary>Hint 1</summary>

Start with a full TCP scan. The obvious services are not the only interesting part of this box.

</details>

<details>
<summary>Hint 2</summary>

There are several low-numbered TCP ports open in a row. Most of them repeat the same themed message, but one of them later becomes important.

</details>

<details>
<summary>Hint 3</summary>

Do not ignore FTP. Try the most common low-friction login method for FTP services.

</details>

---

## First useful file

<details>
<summary>Hint 1</summary>

FTP contains an image. Images in CTF rooms are often more than decoration.

</details>

<details>
<summary>Hint 2</summary>

Look for hidden data inside the image.

</details>

<details>
<summary>Hint 3</summary>

The hidden data does not need a fancy passphrase.

</details>

<details>
<summary>Hint 4</summary>

The extracted content is encoded. Decode it and look closely at the account data.

</details>

---

## Getting a credential lead

<details>
<summary>Hint 1</summary>

The decoded data resembles a Linux shadow file.

</details>

<details>
<summary>Hint 2</summary>

Only one normal user account in that data is really useful.

</details>

<details>
<summary>Hint 3</summary>

The hash type is SHA512 crypt.

</details>

<details>
<summary>Hint 4</summary>

A common leaked-password wordlist is enough for this hash.

</details>

---

## Web foothold

<details>
<summary>Hint 1</summary>

The website has more than a static landing page. Check the source of the interesting PHP page.

</details>

<details>
<summary>Hint 2</summary>

The PHP page contains a form that accepts a command-like field.

</details>

<details>
<summary>Hint 3</summary>

Use the command form to prove server-side execution first.

</details>

<details>
<summary>Hint 4</summary>

Once command execution is confirmed, turn it into a shell.

</details>

<details>
<summary>Hint 5</summary>

Use your VPN interface IP for the callback, not the AttackBox IP unless the AttackBox is the one listening.

</details>

---

## From web user to Charlie

<details>
<summary>Hint 1</summary>

After getting a shell, check who you are and what groups you are in.

</details>

<details>
<summary>Hint 2</summary>

Look around the real user's home directory.

</details>

<details>
<summary>Hint 3</summary>

There is a file with a travel-themed name in the user's home directory.

</details>

<details>
<summary>Hint 4</summary>

That file is not a normal note. Treat it like SSH material.

</details>

<details>
<summary>Hint 5</summary>

When copying the SSH material to your machine, exact formatting matters. The header and footer must be perfect.

</details>

<details>
<summary>Hint 6</summary>

Use the key to log in as the real user named in the public key comment.

</details>

---

## User flag

<details>
<summary>Hint 1</summary>

Once logged in as the real user, check their home directory.

</details>

<details>
<summary>Hint 2</summary>

The user flag is in the expected place.

</details>

---

## Privilege escalation

<details>
<summary>Hint 1</summary>

Check what the real user can run with elevated privileges.

</details>

<details>
<summary>Hint 2</summary>

The interesting sudo rule allows an editor.

</details>

<details>
<summary>Hint 3</summary>

Do not overthink the unusual group restriction at first. Test whether the allowed editor runs without a password.

</details>

<details>
<summary>Hint 4</summary>

Editors often let you escape to a shell.

</details>

<details>
<summary>Hint 5</summary>

GTFOBins has the exact idea needed for this editor.

</details>

---

## A tempting rabbit hole

<details>
<summary>Hint 1</summary>

A writable init script exists and explains the strange low-port listeners.

</details>

<details>
<summary>Hint 2</summary>

The listeners run as root, which looks very interesting.

</details>

<details>
<summary>Hint 3</summary>

This path is not the cleanest route in this room because triggering the service as root is the hard part.

</details>

<details>
<summary>Hint 4</summary>

The sudo editor route is faster and cleaner.

</details>

---

## Root key

<details>
<summary>Hint 1</summary>

Getting root is not quite the end. The root script asks for a key.

</details>

<details>
<summary>Hint 2</summary>

The strange low-port service already hinted where the key lives.

</details>

<details>
<summary>Hint 3</summary>

Look in the web directory for a file with “key” in its name.

</details>

<details>
<summary>Hint 4</summary>

The key file is executable.

</details>

<details>
<summary>Hint 5</summary>

If you are unsure what input it wants, inspect the file as a binary first.

</details>

<details>
<summary>Hint 6</summary>

The output is a Fernet key. Use the key value itself, not the Python byte-string wrapper.

</details>

---

## Final script

<details>
<summary>Hint 1</summary>

The root script uses Python Fernet encryption.

</details>

<details>
<summary>Hint 2</summary>

The key from the executable decrypts the embedded message.

</details>

<details>
<summary>Hint 3</summary>

If the script complains, check whether you pasted extra wrapper characters around the key.

</details>

---

## Route summary

<details>
<summary>Hint 1</summary>

FTP gives an image.

</details>

<details>
<summary>Hint 2</summary>

The image hides encoded account data.

</details>

<details>
<summary>Hint 3</summary>

The decoded data gives a hash for the real user.

</details>

<details>
<summary>Hint 4</summary>

The website gives command execution.

</details>

<details>
<summary>Hint 5</summary>

Command execution gives a web-user shell.

</details>

<details>
<summary>Hint 6</summary>

The web-user shell reveals SSH material for the real user.

</details>

<details>
<summary>Hint 7</summary>

The real user can run an editor with sudo.

</details>

<details>
<summary>Hint 8</summary>

The editor can spawn a root shell.

</details>

<details>
<summary>Hint 9</summary>

The final key comes from the executable in the web directory.

</details>
