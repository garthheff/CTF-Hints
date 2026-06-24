# Anonymous

Room: https://tryhackme.com/room/anonymous

Not the hacking group

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/medium/anonymous.md

---

## Enumeration

<details>
<summary>Hint 1</summary>

Start with a full TCP port scan. There are only a few services exposed, but two of them allow anonymous-style interaction.

</details>

<details>
<summary>Hint 2</summary>

Pay close attention to file-sharing services. One gives you writable access, and another contains files that are mostly useful for confirming the theme or checking for distractions.

</details>

<details>
<summary>Hint 3</summary>

One service exposes a directory related to automated maintenance. The permissions on that directory matter more than the file contents at first glance.

</details>

---

## Initial Foothold

<details>
<summary>Hint 1</summary>

Look for a script that can be modified remotely.

</details>

<details>
<summary>Hint 2</summary>

A log file in the same area gives away that the script is being executed repeatedly.

</details>

<details>
<summary>Hint 3</summary>

Before attempting a shell, modify the script in a harmless way so it writes a unique marker and the execution context into the log.

</details>

<details>
<summary>Hint 4</summary>

Once you confirm the script runs as a real user, replace the harmless test with a callback payload.

</details>

<details>
<summary>Hint 5</summary>

The user you land as is not root, but their group memberships are very useful for alternate escalation paths.

</details>

---

## User Flag

<details>
<summary>Hint 1</summary>

After getting a shell, check the current user’s home directory.

</details>

<details>
<summary>Hint 2</summary>

The user flag follows the normal TryHackMe pattern and is readable by the compromised user.

</details>

---

## Root — Main Path

<details>
<summary>Hint 1</summary>

Start with standard local privilege escalation enumeration.

</details>

<details>
<summary>Hint 2</summary>

Focus on SUID binaries. One binary in the normal system path is unusual to see with SUID enabled.

</details>

<details>
<summary>Hint 3</summary>

The interesting binary is commonly used to run another program with a modified environment.

</details>

<details>
<summary>Hint 4</summary>

Use the SUID binary to launch a shell, and make sure the shell preserves privileges.

</details>

<details>
<summary>Hint 5</summary>

If the prompt changes to a root-style shell, check the effective user ID before reading the root flag.

</details>

---

## Root — Alternate Path

<details>
<summary>Hint 1</summary>

Check the compromised user’s group memberships.

</details>

<details>
<summary>Hint 2</summary>

One group can be root-equivalent if the service is available and can be initialized.

</details>

<details>
<summary>Hint 3</summary>

If no local container images exist, you can prepare a small image elsewhere and transfer it to the target.

</details>

<details>
<summary>Hint 4</summary>

A privileged container can mount the host filesystem inside the container.

</details>

<details>
<summary>Hint 5</summary>

Inside the container, remember that `/root` is the container’s root home. The host filesystem is mounted somewhere else.

</details>

<details>
<summary>Hint 6</summary>

To make the mounted host filesystem behave like the real root filesystem, use a filesystem-root context change.

</details>

</details>

---

## Root — Extra Path

<details>
<summary>Hint 1</summary>

This is an older Linux target, so generic local privilege escalation checks may also apply.

</details>

<details>
<summary>Hint 2</summary>

A well-known PolicyKit vulnerability may work on vulnerable systems.

</details>

<details>
<summary>Hint 3</summary>

This route is not the room-specific intended path. Prefer the SUID path for a cleaner solve.

</details>

---

## Final Checks

<details>
<summary>Hint 1</summary>

There are multiple valid root routes on this box, but the clean intended path is based on misconfigured file permissions and an unusual SUID binary.

</details>

<details>
<summary>Hint 2</summary>

For public notes, describe the alternate root routes as optional validation rather than the main solution.

</details>

<details>
<summary>Hint 3</summary>

Do not publish full flags, full payloads, or direct copy-paste exploit commands in the hints file.

</details>
