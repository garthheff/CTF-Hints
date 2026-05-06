# gamingserver

Room: https://tryhackme.com/room/gamingserver

An Easy Boot2Root box for beginners

Can you gain access to this gaming server built by amateurs with no experience of web development and take advantage of the deployment system.

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/gamingserver.md

---

<details>
<summary>Hint 1</summary>

Start with a full port and service scan. The useful services are minimal, so do not overcomplicate the first step.

Focus on the web service and SSH.

</details>

<details>
<summary>Hint 2</summary>

The website has a standard file that often gives crawler instructions.

Check whether it points to an interesting public directory.

</details>

<details>
<summary>Hint 3</summary>

One of the public directories contains files that are useful later.

Pay attention to anything that looks like a wordlist.

</details>

<details>
<summary>Hint 4</summary>

The page source contains a developer comment.

A name in that comment is useful as a possible Linux username.

</details>

<details>
<summary>Hint 5</summary>

Directory enumeration reveals a hidden-looking location that was not obvious from the homepage.

That location contains something that should not be exposed on a web server.

</details>

<details>
<summary>Hint 6</summary>

The exposed file is an encrypted SSH private key.

Because it is encrypted, having the key alone is not enough. You need to recover its passphrase.

</details>

<details>
<summary>Hint 7</summary>

Convert the encrypted SSH key into a format a password cracker understands.

Use the wordlist found earlier to recover the key passphrase.

</details>

<details>
<summary>Hint 8</summary>

Once the passphrase is recovered, use the private key with the username found in the page source.

This should give you a normal user shell.

</details>

<details>
<summary>Hint 9</summary>

After getting a shell, check the current user's groups.

One group is more useful than the failed sudo path.

</details>

<details>
<summary>Hint 10</summary>

The useful group allows interaction with local containers.

If there are no container images already available, bring in a small Linux image from your attack box.

</details>

<details>
<summary>Hint 11</summary>

A privileged container alone gives root inside the container, but not automatically root over the host filesystem.

You need to attach the host filesystem into the container as a mounted disk.

</details>

<details>
<summary>Hint 12</summary>

After the host filesystem is mounted into the container, the host root directory can be browsed from inside the container.

This is the intended container-based root path.

</details>

---

## Alternate path

<details>
<summary>Hint 13</summary>

The target is an older Ubuntu system with a vulnerable privilege escalation surface.

A known Polkit privilege escalation can also provide root if the target is vulnerable.

</details>

<details>
<summary>Hint 14</summary>

Transfer the exploit binary from your attack box to the target, make it executable, and run it from a writable directory.

This alternate route should drop you into a root shell if the system is vulnerable.

</details>

---
