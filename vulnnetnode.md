# VulnNet: Node

Room: https://tryhackme.com/room/vulnnetnode

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/vulnnetnode.md

After the previous breach, VulnNet Entertainment states it won't happen again. Can you prove they're wrong?

---

## Enumeration

<details>
<summary>Hint 1</summary>

Start with a full TCP scan and pay close attention to the web service. The interesting application is not on the default HTTP port.

</details>

<details>
<summary>Hint 2</summary>

The web service identifies itself as a Node.js Express application.

</details>

<details>
<summary>Hint 3</summary>

Do not spend too long trying to brute force the visible login form. Inspect how it is built and how the browser behaves when you submit it.

</details>

<details>
<summary>Hint 4</summary>

The login page is mostly a distraction. The main page gives more useful clues through cookies, static assets, and article content.

</details>

---

## Web Application Clues

<details>
<summary>Hint 5</summary>

Look at the cookies set by the application. One cookie contains structured data that can be decoded.

</details>

<details>
<summary>Hint 6</summary>

The session value is encoded, not encrypted. Decode it and inspect the fields.

</details>

<details>
<summary>Hint 7</summary>

Try changing harmless values in the decoded session object, then re-encode it and send it back.

</details>

<details>
<summary>Hint 8</summary>

If changing the username changes the page greeting, the server is trusting client-controlled session data.

</details>

---

## Vulnerability Direction

<details>
<summary>Hint 9</summary>

One article on the page specifically points toward Node.js package vulnerabilities.

</details>

<details>
<summary>Hint 10</summary>

The relevant issue is not the Node.js runtime version. Think about how some Node packages handle serialized objects.

</details>

<details>
<summary>Hint 11</summary>

A vulnerable deserialization package can turn a crafted value inside the session object into server-side JavaScript execution.

</details>

<details>
<summary>Hint 12</summary>

Before trying for a shell, prove execution with a harmless value that changes what the page renders.

</details>

---

## Initial Shell

<details>
<summary>Hint 13</summary>

Once code execution is confirmed, use it to execute a simple system command and check which user the web app runs as.

</details>

<details>
<summary>Hint 14</summary>

The command output may look mangled because the page strips characters before rendering it.

</details>

<details>
<summary>Hint 15</summary>

A FIFO based reverse shell works well here if a basic netcat binary is present.

</details>

<details>
<summary>Hint 16</summary>

After catching the shell, upgrade it before doing privilege enumeration.

</details>

---

## First Privilege Pivot

<details>
<summary>Hint 17</summary>

Check what the web user can run with sudo. The allowed command is related to the Node.js package manager.

</details>

<details>
<summary>Hint 18</summary>

The target has an older npm version, so newer npm subcommands may not work.

</details>

<details>
<summary>Hint 19</summary>

npm project scripts can execute shell commands. Use this to pivot to the allowed target user.

</details>

<details>
<summary>Hint 20</summary>

After pivoting, check that user’s home directory for the user flag.

</details>

---

## Root Path

<details>
<summary>Hint 21</summary>

Run sudo checks again as the second user. The allowed commands are related to systemd timer management.

</details>

<details>
<summary>Hint 22</summary>

Inspect the timer unit and follow the service it triggers.

</details>

<details>
<summary>Hint 23</summary>

The important systemd unit files are writable by the second user’s group.

</details>

<details>
<summary>Hint 24</summary>

If you can edit the service that root will run, you can make root perform a controlled action for you.

</details>

<details>
<summary>Hint 25</summary>

The timer’s original schedule may not trigger quickly. Since the timer file is writable, adjust it so it fires shortly after being started.

</details>

<details>
<summary>Hint 26</summary>

Reload systemd after modifying unit files, then stop and start the allowed timer.

</details>

<details>
<summary>Hint 27</summary>

A common final move is to have root create a privileged copy of a shell, then execute it while preserving privileges.

</details>

---

## Final Check

<details>
<summary>Hint 28</summary>

Once you have a root shell, check the root user’s home directory for the final flag.

</details>
