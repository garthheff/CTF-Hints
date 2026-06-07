# Plotted-TMS

Everything here is plotted!

Room: https://tryhackme.com/room/plottedtms

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/plottedtms.md

-----------

<details>
<summary>Hint 1</summary>

Start with a full port scan. Do not make assumptions based only on port numbers.

</details>

<details>
<summary>Hint 2</summary>

One of the open ports looks like it should be SMB, but service detection says otherwise.

</details>

<details>
<summary>Hint 3</summary>

Treat both web ports as HTTP services and enumerate them separately.

</details>

<details>
<summary>Hint 4</summary>

Some paths on port `80` are mostly distractions. The more useful web application is on the unusual HTTP port.

</details>

<details>
<summary>Hint 5</summary>

Look for a `/management` directory.

</details>

<details>
<summary>Hint 6</summary>

Directory listing is enabled in parts of the web application. Do not stop at the login page.

</details>

<details>
<summary>Hint 7</summary>

Check these kinds of directories carefully:

```text
classes
database
uploads
assets
plugins
libs
```

</details>

<details>
<summary>Hint 8</summary>

The database directory contains a useful SQL dump.

</details>

<details>
<summary>Hint 9</summary>

The SQL dump contains application users and password hashes, but the live login may not work with the cracked credentials directly.

</details>

<details>
<summary>Hint 10</summary>

Inspect the login JavaScript to find the real AJAX endpoint.

</details>

<details>
<summary>Hint 11</summary>

The login response leaks the SQL query being used.

</details>

<details>
<summary>Hint 12</summary>

The username field is injectable. You can target the known admin user and comment out the password check.

</details>

<details>
<summary>Hint 13</summary>

A useful login payload is based on this idea:

```text
admin' comment
```

Use the correct SQL comment syntax for the backend.

</details>

<details>
<summary>Hint 14</summary>

Once inside the admin panel, look for file upload functionality. The useful upload is not the quote or About Us text.

</details>

<details>
<summary>Hint 15</summary>

The account avatar upload is useful.

</details>

<details>
<summary>Hint 16</summary>

Uploaded files are stored in a web-accessible uploads directory.

</details>

<details>
<summary>Hint 17</summary>

Try uploading a small PHP command shell as the avatar and then copy the uploaded file URL.

</details>

<details>
<summary>Hint 18</summary>

Use the uploaded PHP file to confirm command execution with a simple command first.

```bash
id
```

</details>

<details>
<summary>Hint 19</summary>

After getting command execution, upgrade it to a reverse shell as the web server user.

</details>

<details>
<summary>Hint 20</summary>

Check the web application configuration files. One file contains database connection details.

</details>

<details>
<summary>Hint 21</summary>

There is a script under `/var/www/scripts`.

</details>

<details>
<summary>Hint 22</summary>

Check the ownership of both the script and the directory containing it. The directory permissions matter.

</details>

<details>
<summary>Hint 23</summary>

Check `/etc/crontab`.

</details>

<details>
<summary>Hint 24</summary>

A cron job runs the backup script every minute as another user.

</details>

<details>
<summary>Hint 25</summary>

Because the scripts directory is writable, you can replace the backup script even if the original script file is owned by another user.

</details>

<details>
<summary>Hint 26</summary>

Test cron execution first by writing `id` output to a temporary file.

</details>

<details>
<summary>Hint 27</summary>

If a SUID shell does not work from `/dev/shm`, check the mount options.

</details>

<details>
<summary>Hint 28</summary>

If `/dev/shm` is mounted with `nosuid`, use a reverse shell from the cron job instead.

</details>

<details>
<summary>Hint 29</summary>

Once you have the user shell, add an SSH key for a stable connection.

</details>

<details>
<summary>Hint 30</summary>

For root, check for `doas`, not just `sudo`.

</details>

<details>
<summary>Hint 31</summary>

Inspect `/etc/doas.conf`.

</details>

<details>
<summary>Hint 32</summary>

The allowed command can read files as root.

</details>

<details>
<summary>Hint 33</summary>

Use the allowed `openssl` command to read the root flag.

</details>

## Final Spoiler

<details>
<summary>Spoiler</summary>

The path is:

```text
Nmap finds SSH and two HTTP services
Port 445 is Apache HTTP, not SMB
/management contains the real app
Directory listing exposes a database dump
The login endpoint leaks SQL queries
SQL injection bypass logs in as admin
Avatar upload allows PHP upload
Uploaded PHP gives command execution as www-data
A writable scripts directory lets you replace a cron backup script
Cron runs that script as plot_admin
Reverse shell gives plot_admin
doas allows plot_admin to run openssl as root
openssl reads /root/root.txt
```

Useful masked payload pattern:

```text
admin' -- -
```

Useful root-read pattern:

```bash
doas openssl enc -in /root/root.txt
```

Flags should be recorded masked, for example:

```text
user: 779275...badb
root: 53f85e...dcab
```

</details>
