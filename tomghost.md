# tomghost — Hints

Identify recent vulnerabilities to try exploit the system or read files that you should not have access to.

Room: https://tryhackme.com/room/tomghost

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/tomghost.md

---



## Getting Started

<details>
<summary>Hint 1</summary>

Start with a full TCP port scan. Do not only scan the top 1000 ports.

</details>

<details>
<summary>Hint 2</summary>

Pay close attention to services that are not just normal HTTP or SSH. One of the discovered ports is closely tied to Tomcat.

</details>

<details>
<summary>Hint 3</summary>

The interesting web service is running on a non-standard web port. Open it in a browser and check what application is being served.

</details>

---

## Web Enumeration

<details>
<summary>Hint 1</summary>

Check the default Tomcat pages and the Manager application paths.

</details>

<details>
<summary>Hint 2</summary>

A `403 Access Denied` on the Manager page does not mean the Manager is absent. It means it exists but access is restricted.

</details>

---

## Service Clue

<details>
<summary>Hint 1</summary>

The exposed service on port `8009` is the key clue.

</details>

<details>
<summary>Hint 2</summary>

Search for vulnerabilities affecting Apache Tomcat 9.0.30 with AJP exposed.

</details>

<details>
<summary>Hint 3</summary>

You are looking for a vulnerability that can read files from the Tomcat web application through AJP.

</details>

---

## The First Useful File

<details>
<summary>Hint 1</summary>

Try reading Tomcat web application metadata files.

</details>

<details>
<summary>Hint 2</summary>

The file you want is commonly found inside `WEB-INF`.

</details>

<details>
<summary>Hint 3</summary>

Try reading:

```text
/WEB-INF/web.xml
```

</details>

<details>
<summary>Hint 4</summary>

The useful information is not a servlet mapping. Look inside the descriptive text.

</details>

---

## First Foothold

<details>
<summary>Hint 1</summary>

The leaked value from the Tomcat metadata is a username and password.

</details>

<details>
<summary>Hint 2</summary>

Use the leaked credentials against SSH.

</details>

<details>
<summary>Hint 3</summary>

After logging in, check the user's home directory for interesting files.

</details>

---

## User Flag

<details>
<summary>Hint 1</summary>

Do not only check your current user's home directory.

</details>

<details>
<summary>Hint 2</summary>

List the `/home` directory and inspect readable files.

</details>

<details>
<summary>Hint 3</summary>

One user's flag can be read from another user's home directory.

</details>

---

## Files of Interest

<details>
<summary>Hint 1</summary>

Two files in the first user's home directory belong together.

</details>

<details>
<summary>Hint 2</summary>

One file is an encrypted credential file. The other is a PGP key.

</details>

<details>
<summary>Hint 3</summary>

Copy both files back to your AttackBox so you can work on them locally.

</details>

---

## Unlocking the Next Step

<details>
<summary>Hint 1</summary>

The PGP key is protected by a passphrase.

</details>

<details>
<summary>Hint 2</summary>

Convert the PGP key into a John-compatible hash.

</details>

<details>
<summary>Hint 3</summary>

On some systems, `gpg2john` needs to be run from its full path.

</details>

<details>
<summary>Hint 4</summary>

Try:

```bash
/usr/share/john/gpg2john.py tryhackme.asc > pgp.hash
```

</details>

<details>
<summary>Hint 5</summary>

Crack the hash with `john` and `rockyou.txt`.

</details>

---

## Decrypting the File

<details>
<summary>Hint 1</summary>

Import the PGP key first.

</details>

<details>
<summary>Hint 2</summary>

Use the cracked passphrase when decrypting the encrypted credential file.

</details>

<details>
<summary>Hint 3</summary>

The decrypted file gives credentials for another local user.

</details>

---

## Second Foothold

<details>
<summary>Hint 1</summary>

Use the newly recovered credentials to SSH in as the second user.

</details>

<details>
<summary>Hint 2</summary>

After logging in, check what the user can run with elevated privileges.

</details>

<details>
<summary>Hint 3</summary>

Run:

```bash
sudo -l
```

</details>

---

## Final Step

<details>
<summary>Hint 1</summary>

The allowed sudo binary is not harmless.

</details>

<details>
<summary>Hint 2</summary>

Search GTFOBins for the allowed binary.

</details>

<details>
<summary>Hint 3</summary>

The useful technique abuses archive testing and a custom unzip command.

</details>

<details>
<summary>Hint 4</summary>

The command can spawn a root shell without needing the root password.

</details>

</details>

---

## Final Flag

<details>
<summary>Hint 1</summary>

Once you have a root shell, check `/root`.

</details>

<details>
<summary>Hint 2</summary>

Read the root flag from the root user's home directory.

</details>
