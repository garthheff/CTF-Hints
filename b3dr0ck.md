# 0x41haz

Simple Reversing Challenge

In this challenge, you are asked to solve a simple reversing solution. Download and analyze the binary to discover the password.

There may be anti-reversing measures in place!

Room: https://tryhackme.com/room/0x41haz

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/0x41haz.md

--------------

## Hint 1

<details>
<summary>Hint 1</summary>

Start with a full port scan. There are a few non-standard services, and the interesting path is not just SSH or the basic web server.

</details>

## Hint 2

<details>
<summary>Hint 2</summary>

The service on the normal web port redirects somewhere more interesting. Make sure you check the HTTPS service running on the higher web port.

</details>

## Hint 3

<details>
<summary>Hint 3</summary>

Read the web page carefully. The important clue mentions something being **OVER 9000**.

</details>

## Hint 4

<details>
<summary>Hint 4</summary>

The clue is pointing you toward the custom service running just over port 9000.

Try connecting to it with `nc`.

</details>

## Hint 5

<details>
<summary>Hint 5</summary>

When the service asks what you are looking for, try asking for certificate-related things.

Useful words to try:

```text
certificate
private
```

</details>

## Hint 6

<details>
<summary>Hint 6</summary>

The service can leak both a certificate and a private key for one of the themed users.

Save them into files, for example:

```bash
barney.crt
barney.key
```

Then make the key readable only by you:

```bash
chmod 600 barney.key
```

</details>

## Hint 7

<details>
<summary>Hint 7</summary>

The service on the highest port is SSL/TLS wrapped and rejects normal connections.

It is not looking for a password first. It wants a client certificate.

</details>

## Hint 8

<details>
<summary>Hint 8</summary>

Use the leaked certificate and key to authenticate to the SSL service.

```bash
openssl s_client -connect TARGET:54321 -cert barney.crt -key barney.key -quiet
```

Once connected, try:

```text
help
```

</details>

## Hint 9

<details>
<summary>Hint 9</summary>

The first authenticated user gets a password hint.

The hint looks like a hash. Once recovered, use it to SSH as the matching lowercase Linux username.

</details>

## Hint 10

<details>
<summary>Hint 10</summary>

After getting a shell as the first user, check sudo permissions.

```bash
sudo -l
```

The allowed binary is not the standard Linux tool it looks like. Check its help output.

</details>

## Hint 11

<details>
<summary>Hint 11</summary>

The custom `certutil` can list and generate certificates.

```bash
certutil ls
certutil -H
```

Because it can be run with sudo, think about generating certificate material for another user.

</details>

## Hint 12

<details>
<summary>Hint 12</summary>

Generate certificate material for the second themed user.

```bash
sudo certutil fred "Fred Flintstone"
```

Save the printed private key and certificate to files.

</details>

## Hint 13

<details>
<summary>Hint 13</summary>

Use the second user’s generated certificate and key against the SSL hint service.

```bash
openssl s_client -connect 127.0.0.1:54321 -cert /tmp/fred.crt -key /tmp/fred.key -quiet
```

Then run:

```text
help
```

</details>

## Hint 14

<details>
<summary>Hint 14</summary>

The second user’s hint gives you their password directly. Use it to switch user or SSH in as that user.

```bash
su - fred
```

</details>

## Hint 15

<details>
<summary>Hint 15</summary>

Check sudo permissions again as the second user.

```bash
sudo -l
```

This user can run specific encoding tools against a root-owned password file.

</details>

## Hint 16

<details>
<summary>Hint 16</summary>

Read `/root/pass.txt` using the exact allowed sudo commands.

```bash
sudo /usr/bin/base32 /root/pass.txt
sudo /usr/bin/base64 /root/pass.txt
```

The output is not the final password yet.

</details>

## Hint 17

<details>
<summary>Hint 17</summary>

The data is encoded multiple times.

The working decode order is:

```text
base32
base32
base64
```

</details>

## Hint 18

<details>
<summary>Hint 18</summary>

After decoding, you should get an MD5 hash.

Crack the hash with your preferred method. In our run, hashes.com was used.

```text
https://hashes.com/en/decrypt/hash
```

</details>

## Hint 19

<details>
<summary>Hint 19</summary>

The cracked value is the root password.

Use:

```bash
su root
```

Then read the final flag from `/root`.

</details>

## Final Spoiler

<details>
<summary>Final Spoiler</summary>

The rough chain is:

```text
4040 web clue
9009 leaks Barney certificate and private key
54321 accepts Barney client certificate
help gives Barney password hint
SSH as barney
sudo certutil generates Fred certificate and key
54321 accepts Fred client certificate
help gives Fred password
switch to Fred
Fred can sudo base32 and base64 against /root/pass.txt
decode base32, base32, then base64
crack the final MD5 hash
su root
read /root/root.txt
```

Masked root flag:

```text
THM{de4...1b7}
```

</details>
