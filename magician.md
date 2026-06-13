# magician

This magical website lets you convert image file formats

Room: https://tryhackme.com/room/magician

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/magician.md

------------

## Initial Enumeration

<details>
<summary>Hint 1</summary>

Add the target IP to `/etc/hosts` using the hostname provided by the room.

</details>

<details>
<summary>Hint 2</summary>

Run a full TCP port scan rather than scanning only the default ports.

</details>

<details>
<summary>Hint 3</summary>

There are three externally reachable services. Pay attention to FTP and both web ports.

</details>

---

## FTP

<details>
<summary>Hint 1</summary>

Try connecting to FTP with anonymous access.

</details>

<details>
<summary>Hint 2</summary>

The FTP service may take a few seconds to complete the login.

</details>

<details>
<summary>Hint 3</summary>

The useful information is displayed during login. You do not need to retrieve files from the FTP server.

</details>

<details>
<summary>Hint 4</summary>

Research the image-processing vulnerability mentioned in the FTP message.

</details>

---

## Web Application

<details>
<summary>Hint 1</summary>

One web port hosts the frontend, while the other appears to host a backend API.

</details>

<details>
<summary>Hint 2</summary>

Inspect the HTML source of the frontend and identify its JavaScript bundles.

</details>

<details>
<summary>Hint 3</summary>

Download the main application JavaScript and search it for URLs and API endpoints.

</details>

<details>
<summary>Hint 4</summary>

Look for strings related to uploading files and listing converted files.

</details>

<details>
<summary>Hint 5</summary>

The frontend communicates with an API on the other web port.

</details>

---

## Understanding the Upload

<details>
<summary>Hint 1</summary>

Upload a normal image through the application before attempting anything malicious.

</details>

<details>
<summary>Hint 2</summary>

Capture the upload request with Burp Suite or your browser's developer tools.

</details>

<details>
<summary>Hint 3</summary>

Note the request method, upload endpoint, multipart field name, filename and content type.

</details>

<details>
<summary>Hint 4</summary>

Observe what happens to a PNG after the server processes it.

</details>

<details>
<summary>Hint 5</summary>

The application is passing uploaded files through an image-conversion utility.

</details>

---

## Initial Foothold

<details>
<summary>Hint 1</summary>

The FTP hint points toward command execution through specially crafted image content.

</details>

<details>
<summary>Hint 2</summary>

The uploader expects an image extension, but the contents of the file do not necessarily have to match that extension.

</details>

<details>
<summary>Hint 3</summary>

Create a crafted MVG payload and save it using an accepted image filename.

</details>

<details>
<summary>Hint 4</summary>

Before trying a reverse shell, use a harmless HTTP callback to confirm that commands execute.

</details>

<details>
<summary>Hint 5</summary>

Start a temporary HTTP server on your AttackBox and make the target request a nonexistent path.

</details>

<details>
<summary>Hint 6</summary>

A `404` response in your HTTP server log is still proof that the target executed the callback command.

</details>

<details>
<summary>Hint 7</summary>

Once command execution is confirmed, host a small reverse-shell script on your AttackBox.

</details>

<details>
<summary>Hint 8</summary>

Use the crafted upload to download that script to the target and execute it.

</details>

---

## User Access

<details>
<summary>Hint 1</summary>

After receiving the reverse shell, spawn a pseudo-terminal to make it easier to use.

</details>

<details>
<summary>Hint 2</summary>

Check the current user and inspect their home directory.

</details>

<details>
<summary>Hint 3</summary>

The user flag is stored in the expected location within the user's home directory.

</details>

---

## Privilege Escalation

<details>
<summary>Hint 1</summary>

There is another interesting file in the user's home directory besides the flag and application files.

</details>

<details>
<summary>Hint 2</summary>

Read the clue carefully. The words “locally listening” and “cat” are important.

</details>

<details>
<summary>Hint 3</summary>

Enumerate services listening only on the loopback interface.

</details>

<details>
<summary>Hint 4</summary>

An unusual service is bound to `127.0.0.1` on a high-numbered port.

</details>

<details>
<summary>Hint 5</summary>

Connecting with Netcat is useful, but the service is not a command shell.

</details>

<details>
<summary>Hint 6</summary>

Try sending a basic HTTP request through Netcat.

</details>

<details>
<summary>Hint 7</summary>

The response reveals a small web application running behind Gunicorn.

</details>

---

## Local Web Service

<details>
<summary>Hint 1</summary>

Inspect the returned HTML instead of relying on the rendered appearance.

</details>

<details>
<summary>Hint 2</summary>

The page contains a POST form with a single important input field.

</details>

<details>
<summary>Hint 3</summary>

The input asks for a filename.

</details>

<details>
<summary>Hint 4</summary>

Test the application using a harmless, world-readable system file.

</details>

<details>
<summary>Hint 5</summary>

The service returns requested file contents in an encoded format.

</details>

<details>
<summary>Hint 6</summary>

Once arbitrary file read is confirmed, consider which protected file contains the final objective.

</details>

<details>
<summary>Hint 7</summary>

Decode the returned value using the standard Base64 command-line utility.

</details>
