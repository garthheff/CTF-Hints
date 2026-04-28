# Jack of All Trades Hints

Room: https://tryhackme.com/room/jackofalltrades

Walthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/jackofalltrades.md

Jack is a man of a great many talents. The zoo has employed him to capture the penguins due to his years of penguin-wrangling experience, but all is not as it seems.

# Jack-of-all-Trades Hints

Room: https://tryhackme.com/room/jackofalltrades

These hints are designed to nudge you forward without giving the whole path away at once. Open each hint only when you need it.

## General approach

<details>
<summary>Hint 1</summary>

Run a full port scan first. Do not assume common services are running on their common ports.

</details>

<details>
<summary>Hint 2</summary>

Pay close attention to the service names and versions from your scan. Something that looks like a web port may not be serving HTTP, and something that looks like an SSH port may not be serving SSH.

</details>

<details>
<summary>Hint 3</summary>

If your browser refuses to open the target web service, the issue may be your browser blocking a normally restricted port rather than the site being down.

</details>

<details>
<summary>Hint 4</summary>

Firefox can block access to some ports that are normally used for non-web services. For a CTF target, check `about:config` and look into `network.security.ports.banned.override`.

</details>

---

## Web enumeration

<details>
<summary>Hint 1</summary>

Once you identify the actual HTTP service, enumerate it with a directory brute-forcer.

</details>

<details>
<summary>Hint 2</summary>

Use extensions while enumerating. This target has useful content that may not show up if you only search for directories.

</details>

<details>
<summary>Hint 3</summary>

Do not only view the rendered page. Read the page source as well.

</details>

<details>
<summary>Hint 4</summary>

Look for comments, odd notes, and any encoded-looking strings in the HTML.

</details>

---

## Encoding and clues

<details>
<summary>Hint 1</summary>

The site gives you a note that looks like encoded data. Start with common encodings before assuming encryption.

</details>

<details>
<summary>Hint 2</summary>

One encoded note on the homepage is Base64.

</details>

<details>
<summary>Hint 3</summary>

The decoded homepage note gives you a password. Keep it for later.

</details>

<details>
<summary>Hint 4</summary>

The recovery page contains another longer encoded string.

</details>

<details>
<summary>Hint 5</summary>

The longer string is not just one layer. Try multiple decoding steps.

</details>

<details>
<summary>Hint 6</summary>

CyberChef is useful here. The chain is Base32, then Hex, then ROT13.

</details>

<details>
<summary>Hint 7</summary>

The decoded message points you back to the homepage and includes a link that should make you think about hiding data inside images.

</details>

---

## Investigation

<details>
<summary>Hint 1</summary>

The obvious image is not always the correct image.

</details>

<details>
<summary>Hint 2</summary>

Download the images from the assets directory and test them locally.

</details>

<details>
<summary>Hint 3</summary>

Use `steghide` against the JPG files.

</details>

<details>
<summary>Hint 4</summary>

The password you decoded from the homepage is useful for extracting hidden data from an image.

</details>

<details>
<summary>Hint 5</summary>

If the dinosaur image gives you a joke or decoy, keep going. Test the other images too.

</details>

<details>
<summary>Hint 6</summary>

The useful hidden file contains credentials for the recovery login.

</details>

---

## Execution

<details>
<summary>Hint 1</summary>

Use the recovered credentials on the recovery page.

</details>

<details>
<summary>Hint 2</summary>

After logging in, you should get access to a page that accepts a command parameter.

</details>

<details>
<summary>Hint 3</summary>

Before throwing a reverse shell at it, test simple commands.

</details>

<details>
<summary>Hint 4</summary>

Check what binaries exist on the target:

```bash
which bash
which python
which sh
```

</details>

<details>
<summary>Hint 5</summary>

URL encode your command when placing it in the query string.

</details>

<details>
<summary>Hint 6</summary>

Start a listener on your AttackBox before triggering the reverse shell.

```bash
nc -lvnp 9001
```

</details>

<details>
<summary>Hint 7</summary>

A Python reverse shell works here if Python is present.

</details>

---

## Getting the user

<details>
<summary>Hint 1</summary>

Once you have a shell as the web user, check `/home`.

</details>

<details>
<summary>Hint 2</summary>

Look for files that are readable by the web user, especially files outside the user's home directory.

</details>

<details>
<summary>Hint 3</summary>

There is a password list available locally.

</details>

<details>
<summary>Hint 4</summary>

SSH is not on its normal port.

</details>

<details>
<summary>Hint 5</summary>

Use Hydra against SSH on the correct port.

```bash
hydra -l jack -P jacks_password_list ssh://TARGET:80 -V
```

</details>

<details>
<summary>Hint 6</summary>

Once you have the password, SSH in using port 80.

```bash
ssh jack@TARGET -p 80
```

</details>

<details>
<summary>Hint 7</summary>

The user flag is not a text file. You may need to copy it back and inspect it visually.

</details>

<details>
<summary>Hint 8</summary>

Use `scp` with the SSH port option if you want to pull the file back:

```bash
scp -P 80 jack@TARGET:/home/jack/user.jpg ./user.jpg
```

</details>

---

## Privilege escalation

<details>
<summary>Hint 1</summary>

Start with the usual checks.

```bash
sudo -l
id
```

</details>

<details>
<summary>Hint 2</summary>

If sudo is not useful, look for SUID binaries.

</details>

<details>
<summary>Hint 3</summary>

Use `find` to list SUID binaries:

```bash
find / -perm -u=s -type f -exec ls -lah {} \; 2>/dev/null
```

</details>

<details>
<summary>Hint 4</summary>

One SUID binary is unusual because it is owned by root but executable by a specific group.

</details>

<details>
<summary>Hint 5</summary>

Check whether your user is in the group allowed to run that binary.

</details>

<details>
<summary>Hint 6</summary>

The interesting binary can read files and print readable strings from them.

</details>

<details>
<summary>Hint 7</summary>

You do not need a root shell to read the final flag if the SUID binary can read the target file for you.

</details>

<details>
<summary>Hint 8</summary>

Try using the unusual SUID binary against the likely root flag path.

</details>

---

## Final nudge

<details>
<summary>Hint 1</summary>

The room is built around misdirection:

- Services are on unexpected ports
- The obvious image is a decoy
- The user flag is hidden in an image
- The privilege escalation path is file reading, not necessarily spawning a root shell

</details>
