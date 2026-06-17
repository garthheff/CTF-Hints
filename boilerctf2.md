# Boiler CTF

Intermediate level CTF. Just enumerate, you'll get there.

Room: https://tryhackme.com/room/boilerctf2

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/medium/boilerctf2.md

---

## Enumeration

<details>
<summary>Hint 1</summary>

Run a full TCP scan. Do not rely only on the default port range.

</details>

<details>
<summary>Hint 2</summary>

One service is running on a very high port.

</details>

<details>
<summary>Hint 3</summary>

Anonymous access is enabled on one of the open services.

</details>

<details>
<summary>Hint 4</summary>

A normal directory listing may not show everything.

</details>

<details>
<summary>Hint 5</summary>

Look for hidden files after logging in anonymously.

</details>

<details>
<summary>Hint 6</summary>

The hidden file contains encoded text.

</details>

<details>
<summary>Hint 7</summary>

The encoding rotates letters through the alphabet.

</details>

---

## Web Enumeration

<details>
<summary>Hint 8</summary>

The default web page is not the only content on the server.

</details>

<details>
<summary>Hint 9</summary>

Check the usual web enumeration files before brute-forcing directories.

</details>

<details>
<summary>Hint 10</summary>

Some of the discovered paths are deliberate distractions.

</details>

<details>
<summary>Hint 11</summary>

Use directory enumeration against the web root.

</details>

<details>
<summary>Hint 12</summary>

A well-known CMS is installed beneath the main web root.

</details>

<details>
<summary>Hint 13</summary>

After finding the CMS, enumerate inside its directory as well.

</details>

<details>
<summary>Hint 14</summary>

Pay attention to directories beginning with an underscore.

</details>

<details>
<summary>Hint 15</summary>

Most unusual directories contain jokes or encoded decoys, but one hosts a real application.

</details>

---

## Hidden Application

<details>
<summary>Hint 16</summary>

The useful application is inside a directory that sounds like a testing area.

</details>

<details>
<summary>Hint 17</summary>

Identify the application name from the page itself.

</details>

<details>
<summary>Hint 18</summary>

Continue enumerating files inside this application directory.

</details>

<details>
<summary>Hint 19</summary>

The interesting file has a three-character name and a three-character extension.

</details>

<details>
<summary>Hint 20</summary>

The file is directly accessible through the browser or with `curl`.

</details>

<details>
<summary>Hint 21</summary>

Read the log carefully. One line contains more information than a normal SSH log entry should.

</details>

---

## Alternative Route

<details>
<summary>Hint 22</summary>

The application also accepts a parameter that is used to choose a graph or report type.

</details>

<details>
<summary>Hint 23</summary>

Try adding a harmless shell command after a command separator.

</details>

<details>
<summary>Hint 24</summary>

The command output may appear inside an HTML dropdown rather than as plain text.

</details>

<details>
<summary>Hint 25</summary>

A reverse shell is possible, but it is not required to complete the room.

</details>

---

## Initial Access

<details>
<summary>Hint 26</summary>

Use the credentials found in the exposed log against the SSH service.

</details>

<details>
<summary>Hint 27</summary>

Remember that SSH is not running on its default port.

</details>

---

## Lateral Movement

<details>
<summary>Hint 28</summary>

Inspect the files in the first user's home directory.

</details>

<details>
<summary>Hint 29</summary>

A shell script contains a commented line that should not have been left there.

</details>

<details>
<summary>Hint 30</summary>

The script also reveals the name of another local user.

</details>

<details>
<summary>Hint 31</summary>

Try reusing the exposed value with that account.

</details>

---

## User Flag

<details>
<summary>Hint 32</summary>

The user flag is not stored in a file named `user.txt`.

</details>

<details>
<summary>Hint 33</summary>

List hidden files in the second user's home directory.

</details>

---

## Privilege Escalation

<details>
<summary>Hint 34</summary>

Check `sudo -l`, but do not assume every listed path is useful.

</details>

<details>
<summary>Hint 35</summary>

One sudo entry is a deliberate dead end.

</details>

<details>
<summary>Hint 36</summary>

Group membership may look promising, but verify that the related service is actually running.

</details>

<details>
<summary>Hint 37</summary>

Enumerate SUID binaries.

</details>

<details>
<summary>Hint 38</summary>

One normally harmless file-searching utility should not be SUID.

</details>

<details>
<summary>Hint 39</summary>

That utility can execute another command.

</details>

<details>
<summary>Hint 40</summary>

Preserve the effective privileges when launching the shell.

</details>

---

## Root Flag

<details>
<summary>Hint 41</summary>

Once privileged, check the root user's home directory.

</details>
