# 🧠 Thompson

boot2root machine for FIT and bsides guatemala CTF
> Expand only if you get stuck.
> Try each step yourself first.

Room: https://tryhackme.com/room/bsidesgtthompson

Walthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/Thompson.md

---

<details>
<summary>Hint 1</summary>

Start with a full port scan.

Make sure you:

* scan all ports
* identify service versions
* don’t rush past anything unusual

</details>

---

<details>
<summary>Hint 2</summary>

Check the web service in your browser.

Try common Tomcat paths.

There are specific endpoints used for administration.

</details>

---

<details>
<summary>Hint 3</summary>

When prompted for credentials, don’t just try guessing.

Cancel the prompt and observe the response carefully.

The page itself gives useful information.

</details>

---

<details>
<summary>Hint 4</summary>

The application tells you where credentials are stored.

It even gives an example.

Use that information directly.

</details>

---

<details>
<summary>Hint 5</summary>

Once authenticated, look at what functionality is available.

Think about how applications get deployed in Tomcat.

There is a feature that allows you to upload something.

</details>

---

<details>
<summary>Hint 6</summary>

If you can upload a file, what kind of file would give you command execution?

Think about payload formats used by Java web servers.

</details>

---

<details>
<summary>Hint 7</summary>

After triggering your payload, you should receive a shell.

It may be limited at first.

Consider upgrading it to something more interactive.

</details>

---

<details>
<summary>Hint 8</summary>

Start enumerating the system manually.

User home directories are a good place to begin.

Look for anything that doesn’t belong or looks unusual.

</details>

---

<details>
<summary>Hint 9</summary>

One file has permissions that are too open.

This should stand out immediately.

Think about why that would be dangerous.

</details>

---

<details>
<summary>Hint 10</summary>

Check what the file actually does.

Also check ownership of any files it interacts with.

This will tell you who is running it.

</details>

---

<details>
<summary>Hint 11</summary>

If a script is executed by a higher-privileged user and you can modify it…

You don’t need to overthink the rest.

</details>

---

<details>
<summary>Hint 12</summary>

Replace the script contents with something that gives you control.

A reverse shell is one option.

There are other ways too.

</details>


