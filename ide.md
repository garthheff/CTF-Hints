# IDE

Room: https://tryhackme.com/room/ide

Walkthrough: https://github.com/garthheff/CTF-Writeups/edit/main/TryHackMe/ide.md

An easy box to polish your enumeration skills!

--------

These hints are ordered from light nudges to stronger spoilers. Flags, passwords, and sensitive values are masked.

## Light Hints

<details>
<summary>Hint 1</summary>

Start with a full TCP port scan. There are more services than just the default web port.

</details>

<details>
<summary>Hint 2</summary>

One service allows anonymous access. Make sure you check hidden entries, not just a normal listing.

</details>

<details>
<summary>Hint 3</summary>

On FTP, use `ls -la`. There is a directory name that is easy to overlook.

</details>

<details>
<summary>Hint 4</summary>

One file has an awkward filename. If a file is named `-`, download it by referencing it as `./-`.

</details>

## Medium Hints

<details>
<summary>Hint 5</summary>

The FTP note gives you a username and points you toward a default password.

</details>

<details>
<summary>Hint 6</summary>

The high web port is running Codiad. The version shown in the page title is useful.

</details>

<details>
<summary>Hint 7</summary>

Try logging into Codiad with the user from the FTP note and the simplest default password.

</details>

<details>
<summary>Hint 8</summary>

The Codiad login endpoint can be tested directly:

```text
/components/user/controller.php?action=authenticate
```

</details>

<details>
<summary>Hint 9</summary>

Once authenticated to Codiad, look for an exploit that matches the exact Codiad version.

</details>

## Stronger Hints

<details>
<summary>Hint 10</summary>

Codiad 2.8.4 has an authenticated RCE exploit available through SearchSploit.

</details>

<details>
<summary>Hint 11</summary>

When using the exploit, include the trailing slash after the port:

```text
http://TARGET-IP:62337/
```

Without the trailing slash, the exploit may build a broken URL.

</details>

<details>
<summary>Hint 12</summary>

The exploit uses two netcat listeners. One sends the payload, and the other catches the real shell.

</details>

<details>
<summary>Hint 13</summary>

After getting a shell as the web user, check the home directory of the user who signed the FTP note.

</details>

<details>
<summary>Hint 14</summary>

Check shell history files. One command leaks the next user's password.

</details>

<details>
<summary>Hint 15</summary>

The leaked password looks like:

```text
Th3d...R3aL
```

Use it to switch to the local user.

</details>

## Privilege Escalation Hints

<details>
<summary>Hint 16</summary>

After becoming the local user, run:

```bash
sudo -l
```

</details>

<details>
<summary>Hint 17</summary>

The user can restart `vsftpd` with sudo.

</details>

<details>
<summary>Hint 18</summary>

The allowed sudo command alone is limited, but check the systemd service file used by `vsftpd`.

</details>

<details>
<summary>Hint 19</summary>

Check permissions on:

```text
/lib/systemd/system/vsftpd.service
```

</details>

<details>
<summary>Hint 20</summary>

The service file is writable by the local user's group. Since the user can restart the service as root, changing the service command gives code execution as root.

</details>

## Final Spoiler Hints

<details>
<summary>Hint 21</summary>

Replace the `vsftpd` systemd unit with a payload that creates a SUID copy of bash:

```bash
printf '%s\n' '[Unit]' 'Description=vsftpd FTP server' 'After=network.target' '' '[Service]' 'Type=oneshot' "ExecStart=/bin/bash -c 'rm -f /tmp/rootbash; cp /bin/bash /tmp/rootbash; chown root:root /tmp/rootbash; chmod 4755 /tmp/rootbash'" 'RemainAfterExit=yes' '' '[Install]' 'WantedBy=multi-user.target' > /lib/systemd/system/vsftpd.service
```

Then reload systemd and restart the service:

```bash
systemctl daemon-reload
sudo /usr/sbin/service vsftpd restart
```

</details>

<details>
<summary>Hint 22</summary>

Use the SUID bash copy to become root:

```bash
/tmp/rootbash -p
id
cat /root/root.txt
```

</details>

## Masked Solution Summary

<details>
<summary>Hint 23</summary>

The intended chain is:

```text
Anonymous FTP
Hidden ... directory
File named -
Codiad credentials from FTP note
Codiad 2.8.4 authenticated RCE
www-data shell
Password leak in /home/drac/.bash_history
su drac
sudo permission to restart vsftpd
Writable vsftpd systemd service
SUID bash
Root
```

</details>
