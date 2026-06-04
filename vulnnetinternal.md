# VulnNet: Internal

VulnNet Entertainment learns from its mistakes, and now they have something new for you...

VulnNet Entertainment is a company that learns from its mistakes. They quickly realized that they can't make a properly secured web application so they gave up on that idea. Instead, they decided to set up internal services for business purposes. As usual, you're tasked to perform a penetration test of their network and report your findings.

    Difficulty: Easy/Medium
    Operating System: Linux

This machine was designed to be quite the opposite of the previous machines in this series and it focuses on internal services. It's supposed to show you how you can retrieve interesting information and use it to gain system access. Report your findings by submitting the correct flags.

Note: It might take 3-5 minutes for all the services to boot.

Room: https://tryhackme.com/room/greprtp

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/vulnnetinternal.md

---

## Enumeration

<details>
<summary>Hint 1</summary>

Start with a full TCP scan. Do not only scan the default top ports.

</details>

<details>
<summary>Hint 2</summary>

There are multiple internal services exposed. Look for file sharing, network storage, and a key-value database.

</details>

<details>
<summary>Hint 3</summary>

The most interesting services are:

- NFS
- rsync
- Redis
- TeamCity

</details>

---

## NFS

<details>
<summary>Hint 1</summary>

Check for NFS exports.

</details>

<details>
<summary>Hint 2</summary>

One exported directory contains configuration files.

</details>

<details>
<summary>Hint 3</summary>

Mount the exported directory locally and inspect it.

</details>

<details>
<summary>Hint 4</summary>

Look closely at Redis configuration.

</details>

---

## Redis

<details>
<summary>Hint 1</summary>

The NFS export leaks a Redis password.

</details>

<details>
<summary>Hint 2</summary>

Use the leaked password to authenticate to Redis.

</details>

<details>
<summary>Hint 3</summary>

List the Redis keys after authenticating.

</details>

<details>
<summary>Hint 4</summary>

One key contains the internal flag.

</details>

<details>
<summary>Hint 5</summary>

Another key contains encoded data that leads to another service.

</details>

---

## Encoded Data

<details>
<summary>Hint 1</summary>

The encoded Redis value is base64.

</details>

<details>
<summary>Hint 2</summary>

Decode it and read the message carefully.

</details>

<details>
<summary>Hint 3</summary>

The decoded message contains credentials for rsync.

</details>

<details>
<summary>Hint 4</summary>

Mask the password in public notes.

Example:

```text
Hcg3...72v
```

</details>

---

## Rsync

<details>
<summary>Hint 1</summary>

List available rsync modules.

</details>

<details>
<summary>Hint 2</summary>

One module hints at home directory access.

</details>

<details>
<summary>Hint 3</summary>

Use the credentials recovered from Redis to access the rsync module.

</details>

<details>
<summary>Hint 4</summary>

Download the exposed files and inspect the user directory.

</details>

---

## User Flag

<details>
<summary>Hint 1</summary>

The user flag is inside the home directory exposed through rsync.

</details>

<details>
<summary>Hint 2</summary>

The username is visible from the downloaded directory structure.

</details>

<details>
<summary>Hint 3</summary>

Mask the flag in public notes.

Example:

```text
THM{da7c...07ab}
```

</details>

---

## Foothold

<details>
<summary>Hint 1</summary>

The rsync exposure can lead to SSH access.

</details>

<details>
<summary>Hint 2</summary>

Look for SSH-related files in the exposed home directory.

</details>

<details>
<summary>Hint 3</summary>

Use the recovered access to log in as the exposed user.

</details>

---

## Local Enumeration

<details>
<summary>Hint 1</summary>

After logging in, enumerate the machine.

Check:

- current user
- sudo permissions
- SUID files
- writable paths
- running processes

</details>

<details>
<summary>Hint 2</summary>

The sudo path is not the main privilege escalation route.

</details>

<details>
<summary>Hint 3</summary>

Look for a service running locally as root.

</details>

---

## TeamCity

<details>
<summary>Hint 1</summary>

A TeamCity server and build agent are running on the target.

</details>

<details>
<summary>Hint 2</summary>

Check the TeamCity build agent configuration.

</details>

<details>
<summary>Hint 3</summary>

The agent configuration contains a token, but it is not the main web login token.

</details>

<details>
<summary>Hint 4</summary>

Search the TeamCity logs for super user authentication tokens.

</details>

<details>
<summary>Hint 5</summary>

Use an empty username and the super user token as the password.

</details>

<details>
<summary>Hint 6</summary>

Mask the token in public notes.

Example:

```text
4579...3701
```

</details>

---

## TeamCity Access

<details>
<summary>Hint 1</summary>

TeamCity runs locally on the target.

</details>

<details>
<summary>Hint 2</summary>

If it is not reachable directly, use SSH port forwarding.

</details>

<details>
<summary>Hint 3</summary>

After logging into TeamCity, create a manual project and build configuration.

</details>

<details>
<summary>Hint 4</summary>

Skip VCS setup. You only need a command-line build step.

</details>

---

## Privilege Escalation

<details>
<summary>Hint 1</summary>

The TeamCity build agent runs commands as root.

</details>

<details>
<summary>Hint 2</summary>

Use a command-line build step to prove execution with `id`.

</details>

<details>
<summary>Hint 3</summary>

Write command output to a temporary file that your SSH user can read.

</details>

<details>
<summary>Hint 4</summary>

The root flag is in root’s home directory.

</details>

<details>
<summary>Hint 5</summary>

For a root shell, have the build step create a SUID copy of bash.

</details>

<details>
<summary>Hint 6</summary>

When running the SUID bash copy, remember to preserve the effective UID.

</details>

---

## Services Flag

<details>
<summary>Hint 1</summary>

There is a services flag on the machine, but it is not found through Redis.

</details>

<details>
<summary>Hint 2</summary>

After gaining root, search for the exact filename.

</details>

<details>
<summary>Hint 3</summary>

The services flag is under an `/opt` share path.

</details>

---
