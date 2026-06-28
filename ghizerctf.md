# Ghizer

lucrecia has installed multiple web applications on the server.

room: https://tryhackme.com/room/ghizerctf

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/medium/ghizerctf.md

---

## Initial Enumeration

<details>
<summary>Hint 1</summary>

Start with full port enumeration. There are multiple web services and at least one Java-related service that becomes important later.

</details>

<details>
<summary>Hint 2</summary>

Do not assume the HTTP and HTTPS services are the same application. Treat them as separate targets.

</details>

<details>
<summary>Hint 3</summary>

One web service is a survey platform. The other is a WordPress installation.

</details>

---

## FTP

<details>
<summary>Hint 1</summary>

Anonymous FTP is available and shows several files.

</details>

<details>
<summary>Hint 2</summary>

Some file names are designed to look very interesting, but they are not the direct path to the real flags.

</details>

<details>
<summary>Hint 3</summary>

Use FTP as context and themeing, but do not spend too long trying to force this path.

</details>

---

## LimeSurvey

<details>
<summary>Hint 1</summary>

Enumerate the survey application directories. Documentation files can reveal useful version information.

</details>

<details>
<summary>Hint 2</summary>

Check whether default administrative credentials are still in use.

</details>

<details>
<summary>Hint 3</summary>

The survey application version is vulnerable to a known authenticated remote code execution issue.

</details>

<details>
<summary>Hint 4</summary>

The public exploit may need minor fixing before it works cleanly with a modern Python interpreter.

</details>

<details>
<summary>Hint 5</summary>

After exploitation, you should have command execution as the web server user.

</details>

---

## Python 3 Exploit Fix

<details>
<summary>Hint 1</summary>

The exploit was originally written for an older Python version.

</details>

<details>
<summary>Hint 2</summary>

Automatic conversion gets most of the way there, but one converted line shadows a built-in function.

</details>

<details>
<summary>Hint 3</summary>

Look for a variable that has the same name as the function used to read command input.

</details>

<details>
<summary>Hint 4</summary>

Rename that variable and update the later reference to it.

</details>

---

## Configuration File

<details>
<summary>Hint 1</summary>

Once you have a shell as the web server user, inspect the survey application files on disk.

</details>

<details>
<summary>Hint 2</summary>

The framework-style directory structure contains an application config directory.

</details>

<details>
<summary>Hint 3</summary>

The database connection settings contain credentials that are reused elsewhere.

</details>

<details>
<summary>Hint 4</summary>

The answer format for this section is `user:password`.

</details>

---

## WordPress Hidden Login

<details>
<summary>Hint 1</summary>

The WordPress site tells you that a login-hiding plugin is installed.

</details>

<details>
<summary>Hint 2</summary>

The hidden login path is not best guessed manually. Find where the plugin stores its setting.

</details>

<details>
<summary>Hint 3</summary>

Check the WordPress configuration file to get database details.

</details>

<details>
<summary>Hint 4</summary>

Query the WordPress options table for the plugin’s stored login slug.

</details>

<details>
<summary>Hint 5</summary>

The final login path is a query-style path beginning with `/?`.

</details>

---

## Web Shell to User

<details>
<summary>Hint 1</summary>

The WordPress route confirms useful information, but it is not necessary to get a second web shell there.

</details>

<details>
<summary>Hint 2</summary>

From the web shell, enumerate local-only listening ports.

</details>

<details>
<summary>Hint 3</summary>

One local-only port is a Java debug interface.

</details>

<details>
<summary>Hint 4</summary>

Use a Java debugger client to attach to the local debug service.

</details>

<details>
<summary>Hint 5</summary>

You need the Java process to pause on a breakpoint before expression evaluation is useful.

</details>

<details>
<summary>Hint 6</summary>

A Log4j watch-related runnable class is a good breakpoint target.

</details>

<details>
<summary>Hint 7</summary>

To avoid quoting problems inside the debugger, place your reverse shell logic in a script on disk, then execute that script from the debugger.

</details>

<details>
<summary>Hint 8</summary>

A successful callback should land as the real desktop user, not the web server user.

</details>

---

## User Flag

<details>
<summary>Hint 1</summary>

After pivoting to the real user, check their home directory.

</details>

<details>
<summary>Hint 2</summary>

The flag file is not readable as the web server user, but it is readable after the Java debug pivot.

</details>

</details>

---

## Privilege Escalation

<details>
<summary>Hint 1</summary>

Always check the real user’s sudo permissions.

</details>

<details>
<summary>Hint 2</summary>

One allowed sudo entry runs a specific Python script as root without requiring a password.

</details>

<details>
<summary>Hint 3</summary>

Inspect the Python script. It imports a standard module from Python’s library.

</details>

<details>
<summary>Hint 4</summary>

Think about Python import order when a script is run from a writable directory.

</details>

<details>
<summary>Hint 5</summary>

Create a local module with the same name as the imported standard module.

</details>

<details>
<summary>Hint 6</summary>

The replacement module should perform a root-owned action, then still provide the function the original script expects.

</details>

<details>
<summary>Hint 7</summary>

A common payload is to create a root-owned SUID shell in a temporary location.

</details>

---

## Root Flag

<details>
<summary>Hint 1</summary>

After the import hijack runs through sudo, check whether your privileged payload was created.

</details>

<details>
<summary>Hint 2</summary>

Run the privileged shell in preserve-privileges mode.

</details>

<details>
<summary>Hint 3</summary>

The root flag is in the standard root directory.

</details>

</details>

---

## Rabbit Holes

<details>
<summary>Hint 1</summary>

The FTP files are tempting but not required for the main solve.

</details>

<details>
<summary>Hint 2</summary>

Writable-looking Ghidra configuration files under the user’s home directory are not the direct privilege escalation path.

</details>

<details>
<summary>Hint 3</summary>

The important Ghidra-related issue is the local Java debug port, not editing the server config.

</details>

</details>
