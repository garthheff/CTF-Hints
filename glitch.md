# GLITCH

Room: https://tryhackme.com/room/glitch

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/glitch.md

Challenge showcasing a web app and simple privilege escalation. Can you find the glitch?

----------------------------

# TryHackMe Glitch Hints

## Access Token

<details>
<summary>Hint 1</summary>

Start with the website source. Look for JavaScript functions that call API endpoints.

</details>

<details>
<summary>Hint 2</summary>

There is an endpoint that returns a token in JSON format.

</details>

<details>
<summary>Hint 3</summary>

The token returned by `/api/access` is base64 encoded.

</details>

<details>
<summary>Hint 4</summary>

Decode the token, then set the decoded value as a cookie named `token`.

</details>

---

## Website Enumeration

<details>
<summary>Hint 1</summary>

After setting the token cookie, reload the page and inspect the JavaScript loaded by the page.

</details>

<details>
<summary>Hint 2</summary>

The main JavaScript file makes a request to another API endpoint.

</details>

<details>
<summary>Hint 3</summary>

The endpoint to investigate is `/api/items`.

</details>

<details>
<summary>Hint 4</summary>

Check which HTTP methods are allowed on `/api/items`.

</details>

---

## Initial Exploitation

<details>
<summary>Hint 1</summary>

The interesting method is not the normal GET request used by the page.

</details>

<details>
<summary>Hint 2</summary>

Try sending POST requests to `/api/items`.

</details>

<details>
<summary>Hint 3</summary>

The vulnerable input is passed in the query string, not in the JSON body.

</details>

<details>
<summary>Hint 4</summary>

Try a parameter named `cmd`.

</details>

<details>
<summary>Hint 5</summary>

A test value that is not valid JavaScript should produce a useful error message.

</details>

<details>
<summary>Hint 6</summary>

The error message points to server-side `eval` in the Node.js route handler.

</details>

<details>
<summary>Hint 7</summary>

Use Node.js `child_process` to prove command execution with something harmless like `whoami` or `id`.

</details>

---

## Reverse Shell

<details>
<summary>Hint 1</summary>

Once command execution works, use it to trigger a reverse shell back to your VPN IP.

</details>

<details>
<summary>Hint 2</summary>

Base64 encode the reverse shell command to avoid quoting issues.

</details>

<details>
<summary>Hint 3</summary>

Use asynchronous `exec`, not `execSync`, for the reverse shell. `execSync` can freeze the web server while the shell is open.

</details>

<details>
<summary>Hint 4</summary>

Start a listener before triggering the payload.

</details>

---

## User Flag

<details>
<summary>Hint 1</summary>

After the reverse shell connects, check which user you are running as.

</details>

<details>
<summary>Hint 2</summary>

The first flag is in the normal home directory for that user.

</details>

<details>
<summary>Hint 3</summary>

Look in `/home/user`.

</details>

---

## Privilege Escalation

<details>
<summary>Hint 1</summary>

There is another user on the system.

</details>

<details>
<summary>Hint 2</summary>

Check the current user's home directory for browser data.

</details>

<details>
<summary>Hint 3</summary>

Firefox profiles can contain saved credentials.

</details>

<details>
<summary>Hint 4</summary>

The important Firefox files are `logins.json`, `key4.db`, and `cert9.db`.

</details>

<details>
<summary>Hint 5</summary>

Transfer the Firefox profile to your machine and decrypt the saved login.

</details>

<details>
<summary>Hint 6</summary>

A tool called `firefox_decrypt` can recover saved Firefox passwords when you have the full profile.

</details>

<details>
<summary>Hint 7</summary>

The recovered credentials belong to the other local user.

</details>

---

## Root

<details>
<summary>Hint 1</summary>

After switching to the other user, enumerate privilege escalation options again.

</details>

<details>
<summary>Hint 2</summary>

Look for `doas` on the system.

</details>

<details>
<summary>Hint 3</summary>

Check `/usr/local/etc/doas.conf`.

</details>

<details>
<summary>Hint 4</summary>

The `doas` configuration allows the second user to run commands as root.

</details>

<details>
<summary>Hint 5</summary>

Use `doas` to spawn a root shell, then read the root flag.

</details>


