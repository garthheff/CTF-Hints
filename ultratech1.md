# UltraTech

The basics of Penetration Testing, Enumeration, Privilege Escalation and WebApp testing

Room: https://tryhackme.com/room/ultratech1

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/medium/ultratech1.md

---

## Enumeration

<details>
<summary>Hint 1</summary>

Do not only scan the default ports. Run a full TCP port scan and inspect every exposed web service.

</details>

<details>
<summary>Hint 2</summary>

There are two unusual web-related ports. One serves the main website, while the other appears to be a Node.js REST API.

</details>

<details>
<summary>Hint 3</summary>

Inspect the main website’s source and JavaScript files. Look for requests sent to the service running on port `8081`.

</details>

<details>
<summary>Hint 4</summary>

The frontend uses two API routes. Their names are related to authentication and network connectivity testing.

</details>

---

## REST API

<details>
<summary>Hint 1</summary>

Try requesting the API routes directly with `curl`.

Pay attention to error messages and missing parameters.

</details>

<details>
<summary>Hint 2</summary>

One route expects a parameter named `login`.

The other expects a parameter representing an IP address.

</details>

<details>
<summary>Hint 3</summary>

The network-testing route passes user-controlled input to a system command.

Consider what could happen if the input is interpreted by a shell.

</details>

<details>
<summary>Hint 4</summary>

Some common shell separators may be removed or filtered.

Command substitution is another way to make a shell execute a command.

</details>

<details>
<summary>Hint 5</summary>

Try placing a harmless command inside backticks:

```text
`command`
```

Use something that clearly identifies the account running the API.

</details>

---

## Database

<details>
<summary>Hint 1</summary>

The API error messages reveal where its source files are stored.

Look around the API’s working directory.

</details>

<details>
<summary>Hint 2</summary>

Search for files commonly associated with SQLite databases.

Useful extensions include:

```text
.db
.sqlite
.sqlite3
```

</details>

<details>
<summary>Hint 3</summary>

Once you identify the database, use command execution to read it or query it with the `sqlite3` utility.

</details>

<details>
<summary>Hint 4</summary>

The database contains a table named `users`.

Inspect its rows for usernames and password hashes.

</details>

---

## Password Cracking

<details>
<summary>Hint 1</summary>

The password values are 32 hexadecimal characters long.

Identify the likely hash type before selecting a cracking mode.

</details>

<details>
<summary>Hint 2</summary>

Try a common password wordlist such as:

```text
/usr/share/wordlists/rockyou.txt
```

</details>

<details>
<summary>Hint 3</summary>

One recovered account name resembles `root`, but uses zeros instead of the letter `o`.

Try the recovered credentials against SSH.

</details>

---

## Privilege Escalation

<details>
<summary>Hint 1</summary>

After logging in, enumerate the user’s identity and group memberships:

```bash
id
groups
```

</details>

<details>
<summary>Hint 2</summary>

One of the user’s supplementary groups controls a container runtime.

Membership in this group can be equivalent to root access.

</details>

<details>
<summary>Hint 3</summary>

Check whether any container images already exist locally:

```bash
docker images
```

Using an existing image avoids needing to download one.

</details>

<details>
<summary>Hint 4</summary>

Docker can mount host directories inside a container.

Consider mounting the host’s root filesystem `/` somewhere inside a container.

</details>

<details>
<summary>Hint 5</summary>

The general structure is:

```bash
docker run --rm -it -v /:/mnt IMAGE
```

The host filesystem will then be visible beneath:

```text
/mnt
```

</details>

<details>
<summary>Hint 6</summary>

The requested file belongs to the root user and is stored in root’s SSH directory.

You only need the first nine characters of the key data, not the entire private key.

</details>
