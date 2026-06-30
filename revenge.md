# Revenge

Room: https://tryhackme.com/room/revenge

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/medium/revenge.md

You've been hired by Billy Joel to get revenge on Ducky Inc...the company that fired him. Can you break into the server and complete your mission?

This is revenge! You've been hired by Billy Joel to break into and deface the Rubber Ducky Inc. webpage. He was fired for probably good reasons but who cares, you're just here for the money. Can you fulfill your end of the bargain?

There is a sister room to this one. If you have not completed Blog yet, I recommend you do so. It's not required but may enhance the story for you.

All images on the webapp, including the navbar brand logo, 404 and 500 pages, and product images goes to Varg. Thanks for helping me out with this one, bud. 

---

## Hint 1

<details>
<summary>Hint 1</summary>

Start with normal service enumeration.

There are only a couple of important exposed services. One of them is a website.

</details>

---

## Hint 2

<details>
<summary>Hint 2</summary>

Fingerprint the web application.

Look for clues such as:

- Web server type
- Page title
- Email addresses
- JavaScript libraries
- Framework hints

</details>

---

## Hint 3

<details>
<summary>Hint 3</summary>

Directory brute forcing with file extensions is useful here.

Do not only search for folders. Also search for common web/app file extensions such as:

- `.txt`
- `.py`
- `.bak`
- `.zip`
- `.html`
- `.js`

</details>

---

## Hint 4

<details>
<summary>Hint 4</summary>

One exposed text file reveals the application’s Python dependencies.

Use the package names to infer what framework and database stack the website is using.

</details>

---

## Hint 5

<details>
<summary>Hint 5</summary>

After finding the dependency file, check for common Flask project files.

The application source is accidentally exposed.

</details>

---

## Hint 6

<details>
<summary>Hint 6</summary>

Read the exposed Flask source carefully.

Look for:

- Database connection strings
- Hardcoded secrets
- Routes
- SQL queries
- Comments from the developer

</details>

---

## Hint 7

<details>
<summary>Hint 7</summary>

One product route passes a URL value directly into a SQL query.

This is the main web vulnerability.

</details>

---

## Hint 8

<details>
<summary>Hint 8</summary>

The vulnerable route uses a product ID in the URL.

Try testing whether changing that value affects the SQL query logic.

A true/false style test can confirm the issue.

</details>

---

## Hint 9

<details>
<summary>Hint 9</summary>

Automated SQL injection tooling can enumerate the database quickly once you identify the vulnerable URL.

Focus on the application database rather than system databases.

</details>

---

## Hint 10

<details>
<summary>Hint 10</summary>

There are multiple interesting tables.

One table contains customer-style data.

Another table contains system-style users.

The system-style users are more useful for getting shell access.

</details>

---

## Hint 11

<details>
<summary>Hint 11</summary>

The password hashes are bcrypt.

Not all bcrypt hashes have the same cost factor.

Prioritise the account with the lower-cost hash.

</details>

---

## Hint 12

<details>
<summary>Hint 12</summary>

After cracking a system user’s hash, try password reuse against exposed remote services.

The username itself is a clue about where it may work.

</details>

---

## Hint 13

<details>
<summary>Hint 13</summary>

Once logged in, check your user ID, groups, and sudo privileges.

Both are relevant.

</details>

---

## Hint 14

<details>
<summary>Hint 14</summary>

The user belongs to a web-related group.

This matters for the final objective because the goal is to modify the website, not simply crash it.

</details>

---

## Hint 15

<details>
<summary>Hint 15</summary>

The sudo permissions allow editing a systemd service file and restarting that service.

A user who can edit a root-controlled service and restart it can usually run something as root.

</details>

---

## Hint 16

<details>
<summary>Hint 16</summary>

Inspect the service file before editing it.

Pay attention to:

- Which user the service normally runs as
- The working directory
- The command started by the service
- How the web app is served

</details>

---

## Hint 17

<details>
<summary>Hint 17</summary>

Privilege escalation can be achieved by changing what the service starts.

A common technique is to make root create a privileged copy of a shell, then execute that copy while preserving privileges.

Do not leave the service broken.

</details>

---

## Hint 18

<details>
<summary>Hint 18</summary>

If the website shows a bad gateway error, the reverse proxy is alive but the backend app is not.

Restore the original service configuration and restart the app service.

</details>

---

## Hint 19

<details>
<summary>Hint 19</summary>

For the actual defacement, do not keep modifying the backend service.

Find the Flask template used for the homepage and make a small visible change.

</details>

---

## Hint 20

<details>
<summary>Hint 20</summary>

The homepage is rendered from a template inside the web app directory.

Look under the application’s `templates` directory.

Make a backup before editing.

</details>

---

## Hint 21

<details>
<summary>Hint 21</summary>

After editing the homepage template, refresh the front page in the browser.

The objective should trigger once the defacement is visible and the site remains online.

</details>

---

## Hint 22

<details>
<summary>Hint 22</summary>

The final flag appears after the mission objective is completed.

Check the compromised host after the front page has been changed.

</details>

---

## High-Level Path

<details>
<summary>Hint 23</summary>

The overall route is:

```text
Web enumeration
→ exposed dependency file
→ exposed Flask source
→ SQL injection in product route
→ dump database
→ crack system user hash
→ SSH access
→ inspect sudo permissions
→ abuse editable systemd service for root
→ restore web service
→ edit homepage template
→ collect final flag
