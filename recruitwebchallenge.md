# Recruit Web Challenge Hints

Infiltrate Recruit's new portal. Map the site, hunt for flaws, and gain unauthorised acces

Recruit has just launched its new recruitment portal, allowing HR staff to manage candidate applications and administrators to oversee hiring decisions. While the platform appears functional, management suspects that security may have been overlooked during development. Your task is to assess the application like a real attacker, mapping its structure, abusing exposed functionality, and exploiting vulnerabilities.

Can you gain an initial foothold, escalate your access, and ultimately log in as the administrator?

Room: https://tryhackme.com/room/recruitwebchallenge

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/recruitwebchallenge.md

---

## Section 1

<details>
<summary>Hint 1</summary>

Start by making sure the hostname resolves correctly. The site behaves more predictably when accessed as the intended virtual host.

```bash
echo 'TARGET_IP recruit.thm' >> /etc/hosts
```

Then browse to:

```text
http://recruit.thm/
```

</details>

<details>
<summary>Hint 2</summary>

Look for common discovery files before brute-forcing too hard.

</details>

<details>
<summary>Hint 3</summary>

a site map maybe? 

</details>

---

## Section 2

<details>
<summary>Hint 1</summary>

A directory listing reveals a useful log file. The log contains an email that explains where one set of credentials was temporarily stored.

</details>

<details>
<summary>Hint 2</summary>

The email gives you a username and tells you where the matching credential lives. Do not brute-force first if the application already tells you where to look.

</details>

<details>
<summary>Hint 3</summary>

The important configuration file is not directly useful when requested through the browser in the normal way, because PHP is executed server-side.

You need to make the file retrieval feature read it as a file.

</details>

---

## Section 3

<details>
<summary>Hint 1</summary>

The API documentation says the CV retrieval service accepts a URL-like value:

```text
/file.php?cv=<URL>
```

Do not assume the useful scheme has to be HTTP or HTTPS.

</details>

<details>
<summary>Hint 2</summary>

If `http://`, `https://`, localhost, and plain filenames all fail, try another URL scheme that points directly at a local file.

</details>

<details>
<summary>Hint 3</summary>

The target file path is likely under the web root.

Think in terms of:

```text
/var/www/html/<filename>
```

</details>

<details>
<summary>Hint 4</summary>

A useful shape for the request is:

```bash
curl -i -G --data-urlencode 'cv=SCHEME:///var/www/html/config.php' 'http://recruit.thm/file.php'
```

Replace `SCHEME` with a wrapper that reads local files.

</details>

---

## Section 4

<details>
<summary>Hint 1</summary>

Once you recover the HR credential, use it on the main login page.

The username is mentioned in the mail log.

</details>

<details>
<summary>Hint 2</summary>

After logging in, carefully inspect the authenticated dashboard. Look for any user-controlled input that appears to query candidate records.

</details>

<details>
<summary>Hint 3</summary>

SQI maybe

</details>

---

## Section 5

<details>
<summary>Hint 1</summary>

SQI of the search feature

</details>

<details>
<summary>Hint 2</summary>

UNION SELECT

</details>

<details>
<summary>Hint 3</summary>

try get database name, table schema, column names
</details>

---

## Final Nudge

<details>
<summary>Hint</summary>

The full chain is:

```text
sitemap discovery
mail log clue
CV retrieval file read
HR login
authenticated search SQL injection
information_schema enumeration
users table dump
administrator login
```

</details>
