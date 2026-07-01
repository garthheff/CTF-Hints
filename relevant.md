# Relevant

Penetration Testing Challenge

Room: https://tryhackme.com/room/relevant

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/medium/relevant.md

---

<details>
<summary>Hint 1</summary>

Start with broad service enumeration. This is a Windows target, so pay close attention to web services, SMB, RDP, and RPC-related ports.

</details>

---

<details>
<summary>Hint 2</summary>

There is more than one HTTP service. Do not only check the standard web port.

</details>

---

<details>
<summary>Hint 3</summary>

SMB is important. Check whether anonymous or guest access reveals any shares.

</details>

---

<details>
<summary>Hint 4</summary>

One non-default SMB share stands out. Its name looks unusual compared with the normal administrative shares.

</details>

---

<details>
<summary>Hint 5</summary>

Look inside the unusual SMB share for a small text file.

</details>

---

<details>
<summary>Hint 6</summary>

The file content is not plain text credentials, but it is encoded using a very common encoding format.

</details>

---

<details>
<summary>Hint 7</summary>

Decode the encoded string locally. It reveals a username and password pair.

Obfuscated example format:

```text
User : P@ssw...123
```

</details>

---

<details>
<summary>Hint 8</summary>

Validate the discovered credentials against SMB. They are useful, but anonymous access may still be enough for part of the path.

</details>

---

<details>
<summary>Hint 9</summary>

Check whether the unusual SMB share is writable. A simple harmless test file is enough.

</details>

---

<details>
<summary>Hint 10</summary>

After writing a test file to the SMB share, check whether the same file is reachable through the high-port IIS service.

</details>

---

<details>
<summary>Hint 11</summary>

The key relationship is:

```text
SMB writable location → IIS-served directory
```

This turns file upload into a possible web shell path.

</details>

---

<details>
<summary>Hint 12</summary>

Because the web server is IIS with ASP.NET support, think about a Windows web payload format rather than a PHP payload.

</details>

---

<details>
<summary>Hint 13</summary>

Upload the web payload to the writable SMB share, then request it through the matching IIS URL path.

</details>

---

<details>
<summary>Hint 14</summary>

Once you have a shell, remember this is Windows. Use Windows file-reading syntax rather than Linux commands.

</details>

---

<details>
<summary>Hint 15</summary>

The user flag is in a normal user profile location.

Obfuscated location pattern:

```text
C:\Users\<user>\Desktop\
```

</details>

---

<details>
<summary>Hint 16</summary>

For privilege escalation, check the current token privileges.

</details>

---

<details>
<summary>Hint 17</summary>

One enabled privilege is the important one:

```text
SeImpersonatePrivilege
```

</details>

---

<details>
<summary>Hint 18</summary>

This Windows Server/IIS context is suitable for a Potato-style impersonation privilege escalation.

</details>

---

<details>
<summary>Hint 19</summary>

A known tool can abuse `SeImpersonatePrivilege` on this target to spawn a higher-privileged command shell.

Tool name hint:

```text
PrintS*******
```

</details>

---

<details>
<summary>Hint 20</summary>

Transfer the privilege escalation tool using the same writable share instead of setting up a separate file server.

</details>

---

<details>
<summary>Hint 21</summary>

Run the impersonation tool and ask it to spawn a command shell. Confirm the result by checking the current identity.

Expected target identity:

```text
nt authority\system
```

</details>

---

<details>
<summary>Hint 22</summary>

The root flag is in the usual Administrator desktop location.

Obfuscated location pattern:

```text
C:\Users\Administrator\Desktop\
```

</details>

---

<details>
<summary>Hint 23</summary>

Final path summary:

```text
SMB enumeration
→ unusual readable share
→ encoded credentials
→ writable share
→ IIS mapping
→ ASP.NET web shell
→ user flag
→ token privilege check
→ impersonation privilege escalation
→ SYSTEM
→ root flag
```

</details>
