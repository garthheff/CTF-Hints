# Archangel Hints

Boot2root, Web exploitation, Privilege escalation, LFI

Room: https://tryhackme.com/room/archangel

Walkthrough: https://github.com/garthheff/CTF-Hints/blob/main/Anonforce.md

## Find a different hostname

<details>
<summary>Hint 1</summary>

Pay close attention to anything on the default web page that looks like a name rather than an IP address.

</details>

<details>
<summary>Hint 2</summary>

A support email address can reveal more than just a mailbox.

</details>

<details>
<summary>Hint 3</summary>

Once you find the hostname, make sure your machine can resolve it locally.

</details>

<details>
<summary>Hint 4</summary>

If the site does not resolve on its own, add the target IP and hostname to your hosts file.

</details>

## Find flag 1

<details>
<summary>Hint 1</summary>

After resolving the correct hostname, revisit the site using that name instead of the raw IP.

</details>

<details>
<summary>Hint 2</summary>

The first flag is not hidden very deeply. It is presented directly on the correct virtual host.

</details>

## Look for a page under development

<details>
<summary>Hint 1</summary>

Start with very standard web enumeration.

</details>

<details>
<summary>Hint 2</summary>

Robots files often reveal paths developers did not intend to advertise.

</details>

<details>
<summary>Hint 3</summary>

Directory brute forcing with common extensions will also help here.

</details>

<details>
<summary>Hint 4</summary>

The page you want is explicitly disallowed in robots.txt.

</details>

## Find flag 2

<details>
<summary>Hint 1</summary>

The development page is not the goal by itself. Look at what it is trying to include.

</details>

<details>
<summary>Hint 2</summary>

A file inclusion point becomes much more useful if you can read the PHP source instead of executing it.

</details>

<details>
<summary>Hint 3</summary>

Think about PHP wrappers that turn a file into readable output.

</details>

<details>
<summary>Hint 4</summary>

If you can get a base64 encoded version of the PHP source, decode it locally and read the logic.

</details>

<details>
<summary>Hint 5</summary>

The next flag is sitting in a comment inside the source code.

</details>

## Get a shell and find the user flag

<details>
<summary>Hint 1</summary>

Read the source carefully. The filter is not doing real path validation. It is only blocking a very specific substring.

</details>

<details>
<summary>Hint 2</summary>

If a blacklist is checking for one exact traversal pattern, try an equivalent traversal that does not contain that exact text.

</details>

<details>
<summary>Hint 3</summary>

You need a file that is both reachable through the LFI and useful for code execution.

</details>

<details>
<summary>Hint 4</summary>

A web server access log can become executable PHP if you can control part of what gets written into it.

</details>

<details>
<summary>Hint 5</summary>

Your browser headers are one place where you fully control content that may be written to the access log.

</details>

<details>
<summary>Hint 6</summary>

Use the inclusion bug to load the Apache access log after poisoning it with PHP.

</details>

<details>
<summary>Hint 7</summary>

Test your payload with a simple command like whoami before trying a full reverse shell.

</details>

<details>
<summary>Hint 8</summary>

Once you have command execution as the web user, go to that user's home directory and read the user flag.

</details>

## Get User 2 flag

<details>
<summary>Hint 1</summary>

From the first shell, do not rush straight into kernel level checks. Enumerate scheduled tasks and writable scripts first.

</details>

<details>
<summary>Hint 2</summary>

System wide cron jobs are often declared in one very standard file.

</details>

<details>
<summary>Hint 3</summary>

Look for a script that runs regularly as another user and check whether you can modify it.

</details>

<details>
<summary>Hint 4</summary>

A reverse shell payload you already know works is often the safest choice to reuse here.

</details>

<details>
<summary>Hint 5</summary>

After the cron job runs, you should land as the next user rather than root.

</details>

<details>
<summary>Hint 6</summary>

The next flag is stored in a directory that already looked more interesting than the rest of the home folder.

</details>

## Root the machine and find the root flag

<details>
<summary>Hint 1</summary>

Inside the interesting directory, one file stands out immediately because of its permissions.

</details>

<details>
<summary>Hint 2</summary>

If you find a SUID binary, inspect it before running random privilege escalation tools.

</details>

<details>
<summary>Hint 3</summary>

Strings output is enough to show what external command the binary relies on.

</details>

<details>
<summary>Hint 4</summary>

If a privileged binary calls a common utility without an absolute path, think about PATH hijacking.

</details>

<details>
<summary>Hint 5</summary>

Create your own version of the called program in a writable directory and place that directory first in PATH.

</details>

<details>
<summary>Hint 6</summary>

When spawning a shell from a SUID context, use a method that preserves privileges.

</details>

<details>
<summary>Hint 7</summary>

Once you pop root, the final flag is exactly where you would expect it to be.

</details>
