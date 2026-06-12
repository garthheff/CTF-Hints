# Soupedecode 01

Test your enumeration skills on this boot-to-root machine.

Soupedecode is an intense and engaging challenge in which players must compromise a domain controller by exploiting authentication, navigating through shares, performing password spraying, and utilizing Pass-the-Hash techniques. Prepare to test your skills and strategies in this multifaceted cyber security adventure.

Note: Please allow 4 minutes for the to properly boot up.

Room: https://tryhackme.com/room/soupedecode01

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/soupedecode01.md

---------

## Initial Enumeration

<details>
<summary>Hint 1</summary>

The target is a Windows domain controller. Focus on the services commonly associated with Active Directory.

</details>

<details>
<summary>Hint 2</summary>

Identify the domain name and hostname from the available network services.

</details>

<details>
<summary>Hint 3</summary>

Add the domain controller and domain name to your local host resolution before continuing.

</details>

---

## SMB

<details>
<summary>Hint 1</summary>

Check whether SMB permits unauthenticated share enumeration.

</details>

<details>
<summary>Hint 2</summary>

A built-in low-privileged account may authenticate without a password.

</details>

<details>
<summary>Hint 3</summary>

Even when the visible shares cannot be read, authenticated RPC access may still reveal useful domain information.

</details>

---

## Domain Accounts

<details>
<summary>Hint 1</summary>

Try enumerating the domain SID and resolving sequential RIDs.

</details>

<details>
<summary>Hint 2</summary>

Impacket includes a lightweight tool that can perform SID lookups without requiring Metasploit.

</details>

<details>
<summary>Hint 3</summary>

Save all entries identified as user accounts into a clean username list.

</details>

---

## First Credentials

<details>
<summary>Hint 1</summary>

There are many generated-looking usernames. Consider a simple password policy mistake rather than a large wordlist attack.

</details>

<details>
<summary>Hint 2</summary>

Test whether users have chosen their own username as their password.

</details>

<details>
<summary>Hint 3</summary>

One account near the beginning of the custom user range uses this weak pattern.

</details>

---

## Authenticated Enumeration

<details>
<summary>Hint 1</summary>

Once you have valid domain credentials, enumerate readable shares again.

</details>

<details>
<summary>Hint 2</summary>

Also check the domain for accounts that have Service Principal Names.

</details>

<details>
<summary>Hint 3</summary>

The next useful account is associated with a file-related service.

</details>

---

## Service Account

<details>
<summary>Hint 1</summary>

Request service tickets for accounts with SPNs.

</details>

<details>
<summary>Hint 2</summary>

The resulting tickets can be cracked offline.

</details>

<details>
<summary>Hint 3</summary>

Use the Hashcat mode intended for Kerberos 5 TGS-REP etype 23 hashes.

</details>

<details>
<summary>Hint 4</summary>

The service account password is present in a common password wordlist.

</details>

---

## Shared Data

<details>
<summary>Hint 1</summary>

Use the recovered service account to enumerate SMB permissions again.

</details>

<details>
<summary>Hint 2</summary>

A previously inaccessible share becomes readable.

</details>

<details>
<summary>Hint 3</summary>

Download and inspect the contents of the backup-related share.

</details>

<details>
<summary>Hint 4</summary>

One file contains credential material for several computer accounts.

</details>

---

## Privilege Escalation

<details>
<summary>Hint 1</summary>

The recovered entries are in a familiar Windows credential-dump format.

</details>

<details>
<summary>Hint 2</summary>

You do not need to crack these hashes before testing them.

</details>

<details>
<summary>Hint 3</summary>

Preserve the trailing dollar sign in computer account names.

</details>

<details>
<summary>Hint 4</summary>

Test each computer account with its matching NTLM hash rather than trying every hash against every account.

</details>

<details>
<summary>Hint 5</summary>

One file-related computer account has administrative access to the domain controller.

</details>

---

## Administrator Access

<details>
<summary>Hint 1</summary>

Use Pass-the-Hash with the privileged computer account.

</details>

<details>
<summary>Hint 2</summary>

Several Impacket remote-execution methods can authenticate using an NTLM hash.

</details>

<details>
<summary>Hint 3</summary>

Once connected, check the usual Desktop locations for both flags.

</details>

---

## Flags

<details>
<summary>User Flag</summary>

The user flag belongs to the weak-password account used for the initial authenticated foothold.

</details>

<details>
<summary>Root Flag</summary>

The root flag is stored on the Administrator's Desktop.

</details>
