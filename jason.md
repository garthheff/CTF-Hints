# jason

Room: https://tryhackme.com/room/jason

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/jason.md

Jax sucks alot.............

In JavaScript everything is a terrible mistake.

We are Horror LLC, we specialize in horror, but one of the scarier aspects of our company is our front-end webserver. We can't launch our site in its current state and our level of concern regarding our cybersecurity is growing exponentially. We ask that you perform a thorough penetration test and try to compromise the root account. There are no rules for this engagement. Good luck!

# Horror LLC Hints

## Hints

<details>
<summary>Hint 1</summary>

Start with a full port scan. There are not many services exposed, so the web service is the main target.

</details>

<details>
<summary>Hint 2</summary>

The website gives you an important clue about the technology being used.

</details>

<details>
<summary>Hint 3</summary>

Directory brute forcing may be noisy because the server responds to random paths with the same page.

Filter the default response length if needed.

</details>

<details>
<summary>Hint 4</summary>

Do not just interact with the form in the browser. View the page source and inspect the JavaScript.

</details>

<details>
<summary>Hint 5</summary>

Pay close attention to how the site creates its cookie.

The expiry time is not normal.

</details>

<details>
<summary>Hint 6</summary>

Try manually sending the cookie value that the JavaScript creates.

Example:

```bash
curl -i http://TARGET/ -H 'Cookie: session=VALUE'
```

</details>

<details>
<summary>Hint 7</summary>

If the page changes to show `guest`, you are on the right track.

</details>

<details>
<summary>Hint 8</summary>

The exact plain text cookie value does not seem to matter much. The important part is that the `session` cookie exists.

</details>

<details>
<summary>Hint 9</summary>

Since the app is built with Node.js, try testing whether the cookie is parsed as encoded structured data.

</details>

<details>
<summary>Hint 10</summary>

Try base64 encoded JSON as the cookie value.

</details>

<details>
<summary>Hint 11</summary>

The useful JSON key is related to the newsletter form.

</details>

<details>
<summary>Hint 12</summary>

A useful test object would look like this:

```json
{"email":"test@example.com"}
```

Base64 encode it and use it as the `session` cookie.

</details>

<details>
<summary>Hint 13</summary>

If the page reflects your email value, the application is trusting client-controlled cookie data.

</details>

<details>
<summary>Hint 14</summary>

This is a Node.js app parsing user-controlled data from a cookie. Think about unsafe deserialization.

</details>

<details>
<summary>Hint 15</summary>

A useful term to research is:

```text
node-serialize _$$ND_FUNC$$_
```

</details>

<details>
<summary>Hint 16</summary>

Before trying commands, prove that the deserialization payload executes by returning a harmless string.

</details>

<details>
<summary>Hint 17</summary>

Once execution is confirmed, use Node.js to execute a system command.

The useful module is:

```text
child_process
```

</details>

<details>
<summary>Hint 18</summary>

Start with simple commands.

```bash
id
whoami
pwd
```

</details>

<details>
<summary>Hint 19</summary>

If command output is messy in the HTML response, encode the command output with base64 and decode it locally.

</details>

<details>
<summary>Hint 20</summary>

After command execution works, use it to get a reverse shell.

</details>

<details>
<summary>Hint 21</summary>

The web application is running from:

```text
/opt/webapp
```

</details>

<details>
<summary>Hint 22</summary>

The first shell should land as the `ubuntu` user.

</details>

<details>
<summary>Hint 23</summary>

Check `/home`. The user flag is not in `ubuntu`'s home directory.

</details>

<details>
<summary>Hint 24</summary>

Look inside the other user's home directory.

</details>

<details>
<summary>Hint 25</summary>

For privilege escalation, always check sudo permissions.

```bash
sudo -l
```

</details>

<details>
<summary>Hint 26</summary>

The privilege escalation is very direct. You do not need a kernel exploit or LXD abuse.

</details>

<details>
<summary>Hint 27</summary>

If sudo allows all commands without a password, become root directly.

```bash
sudo -i
```

</details>

## Spoilers

<details>
<summary>Spoiler 1</summary>

The `session` cookie is a base64 encoded JSON object.

Example:

```json
{"email":"admin@horror.llc"}
```

When encoded and sent as the cookie, the page reflects the email value.

</details>

<details>
<summary>Spoiler 2</summary>

The app is vulnerable to unsafe Node.js deserialization.

A harmless proof-of-concept can use the `_$$ND_FUNC$$_` style payload to return a string such as `pwned`.

</details>

<details>
<summary>Spoiler 3</summary>

Command execution can be achieved with Node.js `child_process`.

The command output is reflected through the newsletter message on the page.

</details>

<details>
<summary>Spoiler 4</summary>

The user flag is located at:

```text
/home/dylan/user.txt
```

Masked flag:

```text
0ba487...af217c
```

</details>

<details>
<summary>Spoiler 5</summary>

The `ubuntu` user has passwordless sudo for all commands.

```bash
sudo -l
sudo -i
```

The root flag is located at:

```text
/root/root.txt
```

Masked flag:

```text
2cd5a9...41760e
```

</details>
