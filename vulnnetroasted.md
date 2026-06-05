# VulnNet: Roasted

Room: https://tryhackme.com/room/vulnnetroasted

VulnNet Entertainment quickly deployed another management instance on their very broad network...

VulnNet Entertainment just deployed a new instance on their network with the newly-hired system administrators. Being a security-aware company, they as always hired you to perform a penetration test, and see how system administrators are performing.

    Difficulty: Easy
    Operating System: Windows

This is a much simpler machine, do not overthink. You can do it by following common methodologies.

Note: It might take up to 6 minutes for this machine to fully boot.

Icon made by DinosoftLabs (opens in new tab) from www.flaticon.com

---

## Light Hints

<details>
<summary>Hint 1</summary>

Look closely at the open ports. If you see DNS, Kerberos, LDAP, SMB, Global Catalog, and WinRM together, you are probably dealing with an Active Directory Domain Controller.

</details>

<details>
<summary>Hint 2</summary>

Start with SMB enumeration. Anonymous access is allowed.

</details>

<details>
<summary>Hint 3</summary>

Do not only focus on default shares. The custom anonymous shares are worth checking first.

</details>

<details>
<summary>Hint 4</summary>

The files in the anonymous shares do not directly give you a login, but they do give useful names and context.

</details>

<details>
<summary>Hint 5</summary>

Anonymous LDAP can show basic naming information, but a full LDAP dump needs valid credentials.

</details>

---

## Medium Hints

<details>
<summary>Hint 6</summary>

Use the names from the anonymous share files to create a username list.

</details>

<details>
<summary>Hint 7</summary>

If every Kerberos username check returns `KDC_ERR_C_PRINCIPAL_UNKNOWN`, your username format is probably wrong.

</details>

<details>
<summary>Hint 8</summary>

Since guest or null SMB authentication works, try RID brute forcing to enumerate real domain users.

Example tool category:

```bash
crackmapexec smb TARGET -u guest -p '' --rid-brute
```

</details>

<details>
<summary>Hint 9</summary>

The real username format is not `firstname.lastname`.

Look for a shorter pattern based on the first initial and surname.

</details>

<details>
<summary>Hint 10</summary>

Once you have real usernames, check whether any account does not require Kerberos pre-authentication.

Example tool category:

```bash
GetNPUsers.py DOMAIN/ -dc-ip TARGET -usersfile users.txt -no-pass
```

</details>

---

## Stronger Hints

<details>
<summary>Hint 11</summary>

One account is AS-REP roastable. It belongs to one of the people mentioned in the anonymous files.

</details>

<details>
<summary>Hint 12</summary>

Use Hashcat mode `18200` for the AS-REP hash.

```bash
hashcat -m 18200 hashfile /usr/share/wordlists/rockyou.txt
```

The recovered password should be masked in notes like:

```text
tj07...89*
```

</details>

<details>
<summary>Hint 13</summary>

The cracked account gives SMB access, but not WinRM access. Use it to check authenticated shares.

</details>

<details>
<summary>Hint 14</summary>

After getting valid domain credentials, check the domain policy and script shares.

</details>

<details>
<summary>Hint 15</summary>

Inside the domain share, look for a scripts folder. One script contains a hardcoded account and password.

</details>

<details>
<summary>Hint 16</summary>

The leaked account has better access than the cracked account.

Mask the credential in notes like:

```text
a-whitehat:bNdK...9ht
```

</details>

---

## Final Spoiler Hints

<details>
<summary>Spoiler 1</summary>

The path to the first useful credential is:

```text
Anonymous SMB shares
Read business and enterprise notes
RID brute force real usernames
AS-REP roast real usernames
Crack the roastable account
```

</details>

<details>
<summary>Spoiler 2</summary>

The WinRM-capable account can access the machine, but the user flag is not on that account's desktop.

Check the `enterprise-core-vn` profile.

Masked flag format:

```text
THM{726...4ed}
```

</details>

<details>
<summary>Spoiler 3</summary>

The system flag is visible on the Administrator desktop, but the file ACL blocks direct reading.

Check permissions with:

```powershell
icacls C:\Users\Administrator\Desktop\system.txt
```

</details>

<details>
<summary>Spoiler 4</summary>

The WinRM account has enough rights to modify the ACL.

Grant the account access to the flag file, then read it.

```powershell
icacls C:\Users\Administrator\Desktop\system.txt /grant "DOMAIN\USER:F"
type C:\Users\Administrator\Desktop\system.txt
```

Masked flag format:

```text
THM{16f...d4c}
```

</details>

---

## Key Takeaways

<details>
<summary>Lessons learned</summary>

- Anonymous SMB can provide enough information to start an AD attack path.
- Names are useful, but real username formats should be confirmed.
- RID brute forcing is valuable when null or guest SMB works.
- AS-REP roasting should be checked once usernames are known.
- Valid SMB credentials do not always mean WinRM access.
- `SYSVOL` is worth checking after any domain credential is found.
- Scripts can leak credentials or operational mistakes.
- File ACLs can block flag access even when the file is visible.
- If your account has the right privileges, modifying the ACL may be the simplest solution.

</details>
