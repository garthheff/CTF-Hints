# HaskHell

Teach your CS professor that his PhD isn't in security.

Room: https://tryhackme.com/room/haskhell

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/medium/haskhell.md

---

## Initial Enumeration

<details>
<summary>Hint 1</summary>

Start with a full port scan. There are not many exposed services, so focus on the unusual one.

</details>

<details>
<summary>Hint 2</summary>

One open port is a normal remote access service. The other is a web service running behind a Python web server.

</details>

<details>
<summary>Hint 3</summary>

Do not spend too long looking for a direct exploit against the web server banner. The interesting part is the application behind it.

</details>

---

## Web Application

<details>
<summary>Hint 1</summary>

Browse the web app manually. The homepage gives strong hints about the theme of the room.

</details>

<details>
<summary>Hint 2</summary>

Look for pages linked from the homepage. One of them describes how student submissions are handled.

</details>

<details>
<summary>Hint 3</summary>

Pay attention to the programming language mentioned in the assignment page.

</details>

<details>
<summary>Hint 4</summary>

One link in the assignment page may not point to the real working upload endpoint. Check the site for another upload form.

</details>

---

## Upload Testing

<details>
<summary>Hint 1</summary>

The upload form expects a multipart file upload.

</details>

<details>
<summary>Hint 2</summary>

The file field name can be discovered directly from the HTML form.

</details>

<details>
<summary>Hint 3</summary>

Generic text files are not the intended file type.

</details>

<details>
<summary>Hint 4</summary>

The app accepts files for the language used in the homework assignment.

</details>

<details>
<summary>Hint 5</summary>

After a valid upload, the server redirects to a route under the uploads area.

</details>

---

## Foothold

<details>
<summary>Hint 1</summary>

The assignment page says submitted files are compiled and executed.

</details>

<details>
<summary>Hint 2</summary>

If you control source code that gets compiled and run on the server, you can make the program do more than solve homework.

</details>

<details>
<summary>Hint 3</summary>

Research how the target language can call operating system commands.

</details>

<details>
<summary>Hint 4</summary>

Use the uploaded program to make the target connect back to your listener.

</details>

<details>
<summary>Hint 5</summary>

Once the uploaded file is accepted, remember that visiting the uploaded file route is what triggers the grading behaviour.

</details>

---

## Post-Exploitation

<details>
<summary>Hint 1</summary>

After getting a shell, stabilize it enough to comfortably enumerate the system.

</details>

<details>
<summary>Hint 2</summary>

Look at the web application source code. It confirms why the exploit worked.

</details>

<details>
<summary>Hint 3</summary>

The first shell runs as the web application user, not the professor.

</details>

<details>
<summary>Hint 4</summary>

Check other home directories. Some files are more readable than they should be.

</details>

---

## User Flag

<details>
<summary>Hint 1</summary>

The user flag is not in the initial web user’s home directory.

</details>

<details>
<summary>Hint 2</summary>

Inspect the professor’s home directory.

</details>

<details>
<summary>Hint 3</summary>

The web user has enough access to read the user flag.

</details>

---

## Pivot

<details>
<summary>Hint 1</summary>

Look carefully at hidden directories inside the professor’s home folder.

</details>

<details>
<summary>Hint 2</summary>

A private authentication file has unsafe permissions.

</details>

<details>
<summary>Hint 3</summary>

Copy the readable private authentication material to your attack machine and fix the local permissions before using it.

</details>

<details>
<summary>Hint 4</summary>

Use it to log in as the professor over the remote access service discovered during enumeration.

</details>

---

## Privilege Escalation

<details>
<summary>Hint 1</summary>

Once logged in as the professor, check what commands can be run with elevated privileges.

</details>

<details>
<summary>Hint 2</summary>

The allowed command is a developer tool for running Python web applications.

</details>

<details>
<summary>Hint 3</summary>

The sudo configuration preserves an environment variable that controls what application file the tool loads.

</details>

<details>
<summary>Hint 4</summary>

If a root-run program imports a file you control, top-level code in that file can execute as root.

</details>

<details>
<summary>Hint 5</summary>

Create a small application file in a writable directory and point the preserved environment variable at it.

</details>

<details>
<summary>Hint 6</summary>

Use the root-run import to create a more convenient root shell path, then execute it with privileges preserved.

</details>

---

## Root Flag

<details>
<summary>Hint 1</summary>

After the privilege escalation works, confirm your effective user ID.

</details>

<details>
<summary>Hint 2</summary>

The final flag is in the usual root-only location.

</details>

---

## Key Lessons

<details>
<summary>Hint 1</summary>

The web server software was not the vulnerability. The grading logic was.

</details>

<details>
<summary>Hint 2</summary>

Compiling and running untrusted student code directly on the host is dangerous.

</details>

<details>
<summary>Hint 3</summary>

Private keys should never be world-readable.

</details>

<details>
<summary>Hint 4</summary>

Preserving dangerous environment variables through sudo can turn harmless-looking allowed commands into root code execution.

</details>
