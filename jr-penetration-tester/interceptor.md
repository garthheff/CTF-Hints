# Interceptor

Use Burp or interception knowledge to modify the traffic and pwn the machine.

MediaHub appears to be a normal internal portal used by journalists to manage content. Everything seems protected behind a login and verification system, but the real story lies in how the application communicates with its backend APIs. 

Your task is to assume the role of an attacker and closely observe traffic between the browser and the server. Using your skills, intercept the requests, analyse how the application processes them, and experiment with modifying the data being sent.

If you understand the flow well enough, a small change in the request might be all it takes to bypass the intended controls. Fire up your , intercept the traffic, and see if you can manipulate the requests to take control of the system.

Room: https://tryhackme.com/room/interceptor

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/interceptor.md

 to a different PHP endpoint than the page itself.

---

## Getting Started

<details>
<summary>Hint 1</summary>

Start with broad service enumeration. Pay attention to anything web related, but do not ignore services that may help explain hostnames or how the box is laid out.

</details>

<details>
<summary>Hint 2</summary>

The web service is the main path forward. Visit it in the browser and note any login, dashboard, upload, or import functionality.

</details>

<details>
<summary>Hint 3</summary>

When web content looks normal, directory and file discovery becomes important. Include common web extensions and also think about files developers accidentally leave behind.

</details>

---

## Web Enumeration

<details>
<summary>Hint 1</summary>

If your directory scan gives too many false positives, compare the size of a fake page with real pages. Filtering by the repeated response length can make the useful results stand out.

</details>

<details>
<summary>Hint 2</summary>

Some forbidden results may not be useful for this route. Focus first on pages that return content or redirect to authentication.

</details>

<details>
<summary>Hint 3</summary>

Once you have a list of discovered web files, use that list as a starting point for looking for backup-style versions of those same files.

</details>

<details>
<summary>Hint 4</summary>

A backup copy of a login-related file is the key discovery. Read it carefully rather than just browsing it.

</details>

---

## Login

<details>
<summary>Hint 1</summary>

The useful information is not hidden in a database at this stage. It is left in a developer note.

</details>

<details>
<summary>Hint 2</summary>

The note does not give the full password directly, but it gives a format. Try the most obvious value that fits the format and the current room context.

</details>

<details>
<summary>Hint 3</summary>

Successful credentials do not take you straight to the dashboard. There is a second verification step.

</details>

---

## Verification Step

<details>
<summary>Hint 1</summary>

Do not only look at the page visually. Watch the browser request made when a verification code is submitted.

</details>

<details>
<summary>Hint 2</summary>

The response tells you more than just whether the code failed. Look for state-related fields in the response.

</details>

<details>
<summary>Hint 3</summary>

Think about whether the server trusts a submitted verification state too much. Try changing what the request says it is submitting, not just the numeric code.

</details>

<details>
<summary>Hint 4</summary>

A browser's network tools are enough to test this. Replaying and editing the verification request is the important idea.

</details>

<details>
<summary>Hint 5</summary>

After the server accepts the verification state, go back to the protected area rather than staying on the verification page.

</details>

---

## Optional Verification Route

<details>
<summary>Hint 1</summary>

There is also a brute-force style route for the verification code, but it is slower and not the cleanest path.

</details>

<details>
<summary>Hint 2</summary>

If brute forcing, the order matters. The likely value is not near the low end of the range.

</details>

</details>

---

## Dashboard Features

<details>
<summary>Hint 1</summary>

After login, inspect the available dashboard features. Uploads are interesting, but the import feature is more important.

</details>

<details>
<summary>Hint 2</summary>

The import feature appears to fetch remote content. Think about what server-side tool might be doing the fetching.

</details>

<details>
<summary>Hint 3</summary>

Do not only test normal web URLs. Try to work out whether the fetcher can be tricked into reading local files.

</details>

<details>
<summary>Hint 4</summary>

The filter is weak. The bypass depends on making the input look acceptable while still causing the server-side fetcher to process something else.

</details>

<details>
<summary>Hint 5</summary>

Once local file reading works, target web-root files first. The user flag is in a simple location under the web directory.

</details>

---

## Getting a Shell

<details>
<summary>Hint 1</summary>

Direct one-liners may cause the application to hang or fail. A staged approach is more reliable.

</details>

<details>
<summary>Hint 2</summary>

Host a small script from your machine, then use the vulnerable import behaviour to make the target retrieve it.

</details>

<details>
<summary>Hint 3</summary>

After the script is on the target, use the same vulnerable behaviour again to execute it.

</details>

<details>
<summary>Hint 4</summary>

Use a listener on your machine before triggering execution. The shell you receive is basic, so expect no job control at first.

</details>

<details>
<summary>Hint 5</summary>

Confirm the user context after the connection lands. The initial shell should be the web server user.

</details>

---

## Final Checks

<details>
<summary>Hint 1</summary>

Keep notes of each finding: exposed backup file, weak OTP verification, server-side import behaviour, local file read, and shell execution.

</details>

<details>
<summary>Hint 2</summary>

When writing the final walkthrough, redact session IDs, flags, passwords, and live IP addresses.

</details>
