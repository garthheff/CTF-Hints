# Overpass

What happens when some broke CompSci students make a password manager?

Obviously a perfect commercial success!

Room: https://tryhackme.com/room/overpass

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/overpass.md

---------------

<details>
<summary>Hint 1 - Where should I start?</summary>

Start with normal web enumeration.

Check the web server, then run directory enumeration to find hidden pages and static files.

</details>

---

<details>
<summary>Hint 2 - What should I look for during web enumeration?</summary>

Look for an admin area and JavaScript files.

The JavaScript files are more important than they first appear.

</details>

---

<details>
<summary>Hint 3 - The login does not work. What now?</summary>

Do not only test the login form manually.

Read the client-side code and understand how the page decides whether you are logged in.

</details>

---

<details>
<summary>Hint 4 - What is wrong with the authentication?</summary>

The authentication check is happening client-side.

The server is not properly validating the session before showing the admin content.

</details>

---

<details>
<summary>Hint 5 - How do I access the admin area?</summary>

The browser is looking for a session cookie.

Give the browser the cookie it expects, then revisit the admin area.

</details>

---

<details>
<summary>Hint 6 - What should I do in the admin area?</summary>

Read the message carefully.

Someone has left something for James because he keeps forgetting passwords.

</details>

---

<details>
<summary>Hint 7 - What is the useful file?</summary>

The useful file is an SSH private key.

Save it locally and fix the permissions before trying to use it.

</details>

---

<details>
<summary>Hint 8 - Why can I not SSH with the key straight away?</summary>

The SSH key is protected with a passphrase.

You need to crack the passphrase first.

</details>

---

<details>
<summary>Hint 9 - What tool helps crack the SSH key?</summary>

Convert the key into a John-compatible hash.

Look at:

```bash
ssh2john.py
```

Then use John with a common wordlist.

</details>

---

<details>
<summary>Hint 10 - What do I do after cracking the key?</summary>

Use the private key and cracked passphrase to SSH as James.

</details>

---

<details>
<summary>Hint 11 - What should I check after logging in?</summary>

Check James' home directory.

There is a user flag and a useful note.

</details>

---

<details>
<summary>Hint 12 - Is there anything else useful as James?</summary>

Yes. Try running the room’s namesake binary.

```bash
overpass
```

Look through the available options.

</details>

---

<details>
<summary>Hint 13 - What does the local password manager reveal?</summary>

It can reveal stored passwords.

This helps show James' poor password handling, but the main privilege escalation hint is elsewhere.

</details>

---

<details>
<summary>Hint 14 - Where is the privilege escalation hint?</summary>

Read the to-do list carefully.

Focus on the note about automated builds and where the builds go.

</details>

---

<details>
<summary>Hint 15 - How are automated builds usually triggered?</summary>

Check for scheduled tasks.

On Linux, cron is a good place to start.

</details>

---

<details>
<summary>Hint 16 - What file should I check?</summary>

Check the system crontab.

```bash
cat /etc/crontab
```

</details>

---

<details>
<summary>Hint 17 - What is dangerous in the cron job?</summary>

There is a root cron job that downloads a script and immediately executes it.

The dangerous pattern is:

```bash
curl something | bash
```

</details>

---

<details>
<summary>Hint 18 - Do I need to edit the cron job?</summary>

No.

You need to control what the cron job downloads.

</details>

---

<details>
<summary>Hint 19 - What part of the cron job can I control?</summary>

Look at the hostname used in the curl command.

If you can change where that hostname resolves, you can make root download from your machine.

</details>

---

<details>
<summary>Hint 20 - Where should I check hostname resolution?</summary>

Check `/etc/hosts`.

```bash
cat /etc/hosts
ls -l /etc/hosts
```

</details>

---

<details>
<summary>Hint 21 - What should I change in /etc/hosts?</summary>

Edit the existing `overpass.thm` entry.

Change it so `overpass.thm` points to your AttackBox IP.

</details>

---

<details>
<summary>Hint 22 - What must exist on my AttackBox?</summary>

The cron job requests a specific path.

Recreate the same directory structure on your AttackBox so the target can request the expected script.

</details>

---

<details>
<summary>Hint 23 - What should the fake script do?</summary>

Serve a script that gives you a shell back.

Make sure your listener is running before cron executes it.

</details>

---

<details>
<summary>Hint 24 - What privilege will the shell have?</summary>

The payload runs as the user defined in the cron job.

Check the crontab line carefully.

</details>

---

<details>
<summary>Hint 25 - Final step</summary>

Once the shell connects back, check your privileges.

Then read the root flag.

</details>

---

## Final Hint Chain

<details>
<summary>Show final hint chain</summary>

```text
Enumerate web
→ Read JavaScript
→ Abuse client-side cookie auth
→ Access admin area
→ Find SSH private key
→ Crack SSH key passphrase
→ SSH as James
→ Read user flag
→ Check Overpass password manager
→ Read todo.txt
→ Find automated build hint
→ Check cron
→ Abuse root curl | bash job
→ Edit existing /etc/hosts entry for overpass.thm
→ Point it to your AttackBox IP
→ Host fake buildscript.sh at the expected path
→ Catch root shell
→ Read root flag
```

</details>
