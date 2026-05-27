# support

Room: https://tryhackme.com/room/support

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/support.md

Pentest the Support Ops platform to exploit vulnerabilities and achieve RCE.

A new internal Support Operations Platform has been deployed to assist IT and helpdesk teams. The application handles user management, internal APIs, and system-level operations. However, security was not the primary focus during development. Several features rely on user-controlled input and weak trust boundaries.

---

## What is the administrator flag displayed on the dashboard?

<details>
<summary>Hint 1</summary>

Start with normal web enumeration. Look for common PHP pages and exposed directories.

</details>

<details>
<summary>Hint 2</summary>

One exposed PHP information page gives away useful session details. Pay attention to cookies, session settings, and where PHP stores session files.

</details>

<details>
<summary>Hint 3</summary>

The login page gives you a likely helpdesk email address. Try a small, sensible password attack against that account rather than starting with a huge brute force.

</details>

<details>
<summary>Hint 4</summary>

After logging in, compare what the application stores server-side versus what it trusts from client-side cookies.

</details>

<details>
<summary>Hint 5</summary>

The IT access cookie is predictable. Work out what value the application expects for a true IT/admin-style cookie.

</details>

<details>
<summary>Hint 6</summary>

The internal user API allows profile lookup by user ID. Try changing the ID to discover other users.

</details>

<details>
<summary>Hint 7</summary>

The theme selector is not just cosmetic. Look at how the selected skin is handled by the dashboard.

</details>

<details>
<summary>Hint 8</summary>

The skin parameter can be abused to read PHP source files inside the web root. Use it to inspect files such as the login page, dashboard, API page, footer, and config.

</details>

<details>
<summary>Hint 9</summary>

The config file contains a useful password clue, but the working admin password is a close variant of it.

</details>

<details>
<summary>Hint 10</summary>

Once you authenticate as the real admin user, the dashboard displays the administrator flag.

</details>

---

## What is the user flag after gaining code execution?

<details>
<summary>Hint 1</summary>

After reading the source, inspect the footer carefully. It contains more than just HTML.

</details>

<details>
<summary>Hint 2</summary>

The footer has a system operation feature that only appears for a real admin session.

</details>

<details>
<summary>Hint 3</summary>

The system operation is restricted, but the restriction only checks how the command starts.

</details>

<details>
<summary>Hint 4</summary>

Try extending the allowed command with command separators.

</details>

<details>
<summary>Hint 5</summary>

Use the command execution to confirm who the web server runs as.

</details>

<details>
<summary>Hint 6</summary>

Turn the command execution into a reverse shell using your AttackBox listener.

</details>

<details>
<summary>Hint 7</summary>

Once you have a shell as the web server user, check common home directories for the user flag.

</details>

---

## Vulnerability chain overview

<details>
<summary>Hint 1</summary>

There are multiple weak trust boundaries. Do not focus on a single bug.

</details>

<details>
<summary>Hint 2</summary>

The useful chain includes weak/default credentials, exposed session information, client-side trust, IDOR, source disclosure, and command injection.

</details>

<details>
<summary>Hint 3</summary>

The forged IT cookie is useful for accessing IT-only functionality, but it does not make the server-side session an admin session.

</details>

<details>
<summary>Hint 4</summary>

The admin-only dashboard section depends on a server-side session variable set during real admin login.

</details>

<details>
<summary>Hint 5</summary>

The source disclosure bug can read files inside the web root, but path canonicalisation prevents reading files outside that directory.

</details>

---

## Things that are likely rabbit holes

<details>
<summary>Hint 1</summary>

SQL injection is unlikely because the login logic compares submitted credentials against a PHP array.

</details>

<details>
<summary>Hint 2</summary>

The exposed PHP session settings are useful for understanding the app, but they do not automatically give you file upload or session-file write access.

</details>

<details>
<summary>Hint 3</summary>

The skin file read bug is constrained to the web root, so direct traversal to files such as the database include or flag file outside the web root is blocked.

</details>

<details>
<summary>Hint 4</summary>

A huge password brute force is probably not intended. Use clues from the leaked source and discovered account names.

</details>
