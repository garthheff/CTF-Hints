# Tech_Supp0rt: 1

Hack into the scammer's under-development website to foil their plans.

Hack into the machine and investigate the target.

Please allow about 5 minutes for the machine to fully boot!

Note: The theme and security warnings encountered in this room are part of the challenge.

Room: https://tryhackme.com/room/techsupp0rt1

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/techsupp0rt1.md


---

## Hints

<details>
<summary>Hint 1</summary>

Start with a full port scan. There are only a few services exposed, but one of them is more useful than it first appears.

</details>

<details>
<summary>Hint 2</summary>

Do not ignore SMB. Try listing shares without credentials.

</details>

<details>
<summary>Hint 3</summary>

One SMB share allows anonymous access and contains a note.

</details>

<details>
<summary>Hint 4</summary>

The useful SMB share is related to the web server.

</details>

<details>
<summary>Hint 5</summary>

The note gives you credentials, but the password is not plain text.

</details>

<details>
<summary>Hint 6</summary>

The phrase about being “cooked” is a hint toward CyberChef.

</details>

<details>
<summary>Hint 7</summary>

CyberChef Magic can decode the Subrion password. The chain involves multiple base encodings.

</details>

<details>
<summary>Hint 8</summary>

The public Subrion path is broken because it redirects to the wrong internal IP address. Try the admin panel directly instead.

</details>

<details>
<summary>Hint 9</summary>

The working Subrion path is the panel path, not the public site path.

</details>

<details>
<summary>Hint 10</summary>

Once logged into Subrion, check the version and search for known authenticated exploits.

</details>

<details>
<summary>Hint 11</summary>

The useful Subrion issue is not XSS or CSRF. Look for an authenticated file upload vulnerability.

</details>

<details>
<summary>Hint 12</summary>

The Exploit-DB script for Subrion CMS 4.2.1 arbitrary file upload can give command execution as the web server user.

</details>

<details>
<summary>Hint 13</summary>

If a basic bash reverse shell fails, try a Python socket reverse shell instead.

</details>

<details>
<summary>Hint 14</summary>

After getting a shell as the web user, search web application config files for database credentials.

</details>

<details>
<summary>Hint 15</summary>

The first database password you find may not be reused by the Linux user. Keep looking.

</details>

<details>
<summary>Hint 16</summary>

The original SMB note mentioned WordPress. Find the WordPress config file.

</details>

<details>
<summary>Hint 17</summary>

The WordPress config contains a password that can be reused to become the local user.

</details>

<details>
<summary>Hint 18</summary>

The local user is named after the scam site theme.

</details>

<details>
<summary>Hint 19</summary>

Once you become the local user, always check sudo permissions.

</details>

<details>
<summary>Hint 20</summary>

The allowed sudo command is listed in GTFOBins and can be abused to read or write files as root.

</details>

---

## Stronger Hints

<details>
<summary>Hint 21</summary>

Use anonymous SMB access to read `enter.txt` from the `websvr` share.

</details>

<details>
<summary>Hint 22</summary>

The encoded Subrion password decodes with CyberChef Magic using Base58, Base32, then Base64.

</details>

<details>
<summary>Hint 23</summary>

The Subrion admin panel is available at:

```text
/subrion/panel/
```

</details>

<details>
<summary>Hint 24</summary>

SearchSploit has the exploit you need:

```text
Subrion CMS 4.2.1 - Arbitrary File Upload
```

</details>

<details>
<summary>Hint 25</summary>

The exploit gives a shell as:

```text
www-data
```

</details>

<details>
<summary>Hint 26</summary>

Look for:

```text
wp-config.php
```

The password inside it is the one that works for the local user.

</details>

<details>
<summary>Hint 27</summary>

The local user is:

```text
scamsite
```

</details>

<details>
<summary>Hint 28</summary>

Run:

```bash
sudo -l
```

The useful command is:

```text
/usr/bin/iconv
```

</details>

<details>
<summary>Hint 29</summary>

`iconv` can read `/root/root.txt` directly when run with sudo.

</details>

<details>
<summary>Hint 30</summary>

Instead of only reading the root flag, `iconv` can also be used to write a sudoers drop-in and get a full root shell.

</details>

---

## Final Spoiler

<details>
<summary>Full Path</summary>

The route is:

```text
Nmap finds SSH, HTTP, and SMB
Anonymous SMB access exposes the websvr share
websvr contains enter.txt
enter.txt leaks encoded Subrion admin credentials
CyberChef Magic decodes the password
/subrion redirects to a broken internal IP
/subrion/panel/ works
Login to Subrion as admin
Exploit Subrion CMS 4.2.1 arbitrary file upload
Get RCE as www-data
Use a Python reverse shell if bash fails
Search web configs
Subrion DB password does not work for user reuse
WordPress wp-config.php exposes another password
Use that password to become scamsite
Run sudo -l
Abuse NOPASSWD iconv
Read root flag or write sudoers for a root shell
```

Masked key credentials:

```text
Subrion: admin:Scam...2021
WordPress DB password: ImAS...123!
Root flag: 851b...790b
```

</details>
