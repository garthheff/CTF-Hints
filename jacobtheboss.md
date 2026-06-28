# Jacob the Boss

Find a way in and learn a little more.

Room: https://tryhackme.com/room/jacobtheboss

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/medium/jacobtheboss.md

---

## Initial Access

<details>
<summary>Hint 1</summary>

Start with a full TCP port scan. There are a lot of open ports, but many of them are related to the same technology stack.

</details>

<details>
<summary>Hint 2</summary>

Do not treat every open Java-related port as a separate rabbit hole. Look for a hostname or service banner that ties them together.

</details>

<details>
<summary>Hint 3</summary>

The room text gives an important setup step involving a hostname. Make sure your machine can resolve it before spending too much time on web enumeration.

</details>

<details>
<summary>Hint 4</summary>

Port 80 has a blog/CMS, but it is not the most direct foothold. Keep it in mind, but focus on the Java application server.

</details>

<details>
<summary>Hint 5</summary>

The application server exposes management interfaces that should not be publicly reachable.

</details>

<details>
<summary>Hint 6</summary>

Look for a management console that lets you inspect MBeans. You are looking for functionality related to deployment or file storage.

</details>

<details>
<summary>Hint 7</summary>

One deployment route may fail or throw server errors. There is another MBean that can write files into deployed web applications.

</details>

<details>
<summary>Hint 8</summary>

The useful MBean has a method that stores content using arguments similar to:

```text
folder
filename
extension
content
boolean option
```

</details>

<details>
<summary>Hint 9</summary>

Writing into the first obvious console application may appear successful but still not be reachable from the browser. Try the web console application path instead.

</details>

<details>
<summary>Hint 10</summary>

The payload should be a small JSP page that executes a command passed through a request parameter.

Avoid relying only on automated tooling here; manually understanding the MBean write primitive makes the foothold much clearer.

</details>

<details>
<summary>Hint 11</summary>

Once the JSP is reachable, test with a harmless command first to confirm which user the application server is running as.

</details>

<details>
<summary>Hint 12</summary>

After command execution is confirmed, use the JSP to obtain a reverse shell as the application user.

</details>

---

## User Flag

<details>
<summary>Hint 1</summary>

After getting a shell, check the current user and their home directory.

</details>

<details>
<summary>Hint 2</summary>

The user flag is in the expected place for the compromised Linux user.

</details>

---

## Privilege Escalation

<details>
<summary>Hint 1</summary>

Start with standard local enumeration. Pay special attention to SUID binaries.

</details>

<details>
<summary>Hint 2</summary>

There is a custom SUID binary that does not belong to a normal base Linux install.

</details>

<details>
<summary>Hint 3</summary>

Inspect the custom binary with basic static analysis. Look for strings that show how it builds and runs a system command.

</details>

<details>
<summary>Hint 4</summary>

The binary appears to wrap a network diagnostic command.

</details>

<details>
<summary>Hint 5</summary>

The dangerous part is not the diagnostic tool itself. The issue is how user input is passed into a shell command.

</details>

<details>
<summary>Hint 6</summary>

Test whether shell metacharacters are interpreted. Use a harmless command first.

</details>

<details>
<summary>Hint 7</summary>

If your injected command runs with elevated privileges, use that to spawn or create a root shell.

</details>

<details>
<summary>Hint 8</summary>

A privilege-preserving shell is useful here.

</details>

---

## Final Nudge

<details>
<summary>Hint 1</summary>

The overall path is:

```text
hostname setup
-> exposed application server management
-> write a JSP into a deployed web console
-> shell as the application user
-> custom SUID helper
-> command injection
-> root
```

</details>
