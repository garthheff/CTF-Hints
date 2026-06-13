# Madness

Will you be consumed by Madness?

Room: https://tryhackme.com/room/madness

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/madness.md

-------------

## Initial Access

<details>
<summary>Hint 1</summary>

Start with a full port scan and check the web service carefully.

</details>

<details>
<summary>Hint 2</summary>

The default Apache page is not the end of the web enumeration.

Inspect the page source and look for referenced files.

</details>

<details>
<summary>Hint 3</summary>

There is an image on the web server that does not behave like a normal JPEG.

Check it with:

```bash
file thm.jpg
xxd thm.jpg | head
```

</details>

<details>
<summary>Hint 4</summary>

Compare the beginning of the file with the normal magic bytes for a JPEG.

A JPEG should begin with:

```text
ff d8 ff
```

</details>

<details>
<summary>Hint 5</summary>

The damaged header can be repaired.

You may need to prepend the correct bytes and skip part of the original header.

</details>

<details>
<summary>Hint 6</summary>

Once repaired, open the image.

It contains a clue to a hidden web directory.

</details>

<details>
<summary>Hint 7</summary>

The hidden page accepts a GET parameter named:

```text
secret
```

Try supplying small numeric values.

</details>

<details>
<summary>Hint 8</summary>

The valid value is within a small range.

A short `curl` loop is enough:

```bash
for i in $(seq 0 99); do
    curl -s "http://<TARGET_IP>/<DISCOVERED_PATH>/?secret=$i" |
        grep -q "wrong" || echo "[+] $i"
done
```

</details>

<details>
<summary>Hint 9</summary>

The successful response gives you something useful for the repaired image.

Think steganography.

</details>

<details>
<summary>Hint 10</summary>

Use:

```bash
steghide extract -sf fixed.jpg
```

</details>

<details>
<summary>Hint 11</summary>

The extracted username is not immediately readable.

It uses a very simple substitution cipher where letters are rotated by 13 places.

</details>

<details>
<summary>Hint 12</summary>

Decode it with:

```bash
echo '<ENCODED_USERNAME>' | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

</details>

<details>
<summary>Hint 13</summary>

The username is only half of the SSH credentials.

There is another image associated with the room.

</details>

<details>
<summary>Hint 14</summary>

Download the second image and inspect it with `steghide`.

```bash
steghide info 5iW7kC8.jpg
steghide extract -sf 5iW7kC8.jpg
```

</details>

<details>
<summary>Hint 15</summary>

The second image does not require a passphrase.

The extracted file contains what you need for SSH.

</details>

---

## Privilege Escalation

<details>
<summary>Hint 1</summary>

Start with the usual checks:

```bash
sudo -l
find / -perm -4000 -type f 2>/dev/null
```

</details>

<details>
<summary>Hint 2</summary>

One SUID binary stands out because it includes a version number in its filename.

</details>

<details>
<summary>Hint 3</summary>

Look closely at:

```text
/bin/screen-4.5.0
```

</details>

<details>
<summary>Hint 4</summary>

Search for local privilege-escalation vulnerabilities affecting GNU Screen 4.5.0 when installed SUID root.

</details>

<details>
<summary>Hint 5</summary>

The relevant vulnerability is:

```text
CVE-2017-5618
```

</details>

<details>
<summary>Hint 6</summary>

The exploit abuses Screen’s logfile handling to write to:

```text
/etc/ld.so.preload
```

</details>

<details>
<summary>Hint 7</summary>

You need two compiled files:

* A shared library that changes ownership and permissions on a shell
* A shell that preserves elevated privileges

</details>

<details>
<summary>Hint 8</summary>

Compile the shared object with:

```bash
gcc -fPIC -shared -o /tmp/libhax.so /tmp/libhax.c
```

Compile the shell with:

```bash
gcc -o /tmp/rootshell /tmp/rootshell.c
```

</details>

<details>
<summary>Hint 9</summary>

Run the vulnerable Screen binary from `/etc` with a permissive umask and use its logfile option to create `ld.so.preload`.

</details>

<details>
<summary>Hint 10</summary>

After triggering Screen, check:

```bash
ls -l /tmp/rootshell
```

You are looking for:

```text
-rwsr-xr-x 1 root root ...
```

</details>

<details>
<summary>Hint 11</summary>

If the shell is owned by root and has the SUID bit set, run it directly:

```bash
/tmp/rootshell
```

</details>

<details>
<summary>Hint 12</summary>

Verify with:

```bash
whoami
id
```

Then check the root user’s home directory.

</details>
