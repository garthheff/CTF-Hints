# Gallery

Try to exploit our image gallery system

Our gallery is not very well secured.

Designed and created by Mikaa

Room: https://tryhackme.com/room/gallery666

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/gallery666.md

--------

# Gallery Hints

## Hint 1

<details>
<summary>Hint 1</summary>

Start with a full port scan. There are only a few open ports, and the web service is the main attack surface.

</details>

## Hint 2

<details>
<summary>Hint 2</summary>

Enumerate the web server carefully. The interesting application is not at the root path.

</details>

## Hint 3

<details>
<summary>Hint 3</summary>

Directory brute forcing is useful here. Look for common PHP files and folders.

</details>

## Hint 4

<details>
<summary>Hint 4</summary>

The application exposes its name and version in the page source/footer.

</details>

## Hint 5

<details>
<summary>Hint 5</summary>

Once you know the CMS name and version, check for known public exploits.

</details>

## Hint 6

<details>
<summary>Hint 6</summary>

The public exploit requires authenticated access, but the login page itself is weak.

</details>

## Hint 7

<details>
<summary>Hint 7</summary>

The login endpoint leaks useful SQL query information when incorrect credentials are submitted.

</details>

## Hint 8

<details>
<summary>Hint 8</summary>

A simple SQL injection payload in the username field can bypass the login page.

</details>

## Hint 9

<details>
<summary>Hint 9</summary>

After logging in, focus on album image pages. The vulnerable parameter is an `id` value in the URL.

</details>

## Hint 10

<details>
<summary>Hint 10</summary>

Use SQLmap with your authenticated session cookie against the album image URL.

</details>

## Hint 11

<details>
<summary>Hint 11</summary>

Dump the application database and inspect the `users` table.

</details>

## Hint 12

<details>
<summary>Hint 12</summary>

The dumped admin hash answers one of the room questions, but cracking it is not required to continue.

</details>

## Hint 13

<details>
<summary>Hint 13</summary>

Look at where avatar files are stored. The upload path is useful for getting code execution.

</details>

## Hint 14

<details>
<summary>Hint 14</summary>

Try uploading a PHP shell through the avatar upload, then browse to the uploaded file under `/gallery/uploads/`.

</details>

## Hint 15

<details>
<summary>Hint 15</summary>

Once command execution works, use the web shell to get a reverse shell as `www-data`.

</details>

## Hint 16

<details>
<summary>Hint 16</summary>

The user flag belongs to `mike`, but `www-data` cannot read it directly.

</details>

## Hint 17

<details>
<summary>Hint 17</summary>

Check backup locations on the system. There is a backup related to Mike’s home directory.

</details>

## Hint 18

<details>
<summary>Hint 18</summary>

Review shell history files inside the backup. One command accidentally contains useful credential material.

</details>

## Hint 19

<details>
<summary>Hint 19</summary>

Use the recovered credential to switch to `mike`, then check the user flag and sudo permissions.

</details>

## Hint 20

<details>
<summary>Hint 20</summary>

Mike can run a script in `/opt` with sudo. One of the script options opens a file in an editor.

</details>

## Hint 21

<details>
<summary>Hint 21</summary>

If an editor is opened with root privileges, check whether it supports command execution.

</details>

## Hint 22

<details>
<summary>Hint 22</summary>

In `nano`, command execution can be reached with `Ctrl+r`, then `Ctrl+x`.

</details>

## Final Spoiler

<details>
<summary>Final Spoiler</summary>

The path is:

1. Find `/gallery`.
2. Identify **Simple Image Gallery System v1.0**.
3. Bypass login with SQL injection using the username payload `' OR 1=1 -- -`.
4. Use SQLmap on `/gallery/?page=albums/images&id=2` with the authenticated PHP session cookie.
5. Dump `gallery_db.users` and recover the admin hash.
6. Upload a PHP shell through the avatar upload.
7. Execute the shell from `/gallery/uploads/`.
8. Get a reverse shell as `www-data`.
9. Find `/var/backups/mike_home_backup/.bash_history`.
10. Recover Mike’s password from the history file.
11. `su mike`.
12. Run `sudo /bin/bash /opt/rootkit.sh`.
13. Choose `read`.
14. Use `nano` command execution to set SUID on `/bin/bash`.
15. Run `/bin/bash -p` for root.

Mask flags and passwords in your notes.

</details>
