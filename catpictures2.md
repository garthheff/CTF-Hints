# Cat Pictures 2 

Room: https://tryhackme.com/room/catpictures2

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/catpictures2.md

Now with more Cat Pictures!

# Cat Pictures 2 Hints

---

## Flag 1

<details>
<summary>Hint 1</summary>

Start with broad service enumeration. There is more than one web application running.

</details>

<details>
<summary>Hint 2</summary>

One web app is a photo gallery. Treat it like a CTF target, not just a website to browse.

</details>

<details>
<summary>Hint 3</summary>

Download the full-size images from the album, not only the thumbnails.

</details>


<details>
<summary>Hint 4</summary>

Check image metadata carefully.

</details>

<details>
<summary>Hint 5</summary>

After logging in to the Git service, check the repository contents for the first flag.

</details>

---

## Flag 2

<details>
<summary>Hint 1</summary>

The room gives a strong hint for this stage: Ansible.

</details>

<details>
<summary>Hint 2</summary>

Look for a service that provides buttons or actions for running predefined jobs.

</details>

<details>
<summary>Hint 3</summary>

The action list is more useful than the web UI alone.

</details>

<details>
<summary>Hint 4</summary>

Not every action is equally useful. One action has strict argument validation, so obvious command injection is not the intended path.

</details>

<details>
<summary>Hint 5</summary>

Another action runs a playbook and shows output from it.

</details>

<details>
<summary>Hint 6</summary>

When the playbook action runs, notice what happens before the playbook starts.

</details>

<details>
<summary>Hint 7</summary>

The playbook is being pulled from a Git repository.

</details>

<details>
<summary>Hint 8</summary>

The credentials from the first stage give access to the repository that controls the playbook.

</details>

<details>
<summary>Hint 9</summary>

You do not need Gitea SSH for the final route. Editing through the web UI is enough if you can commit changes.

</details>

<details>
<summary>Hint 10</summary>

If your task runs but you cannot see the command output, remember that Ansible usually needs a variable to capture output and a debug task to print it.

</details>

<details>
<summary>Hint 11</summary>

Make a small harmless change first to prove your edited playbook is being pulled and executed.

</details>

<details>
<summary>Hint 12</summary>

Once you can run a command through the playbook, use it to get a shell as the user running the deployment.

</details>

<details>
<summary>Hint 13</summary>

The second flag is in that user's home directory.

</details>

---

## Root route A

This is the common public walkthrough style route.

<details>
<summary>Hint 1</summary>

After getting the user shell, run normal Linux privilege escalation checks.

</details>

<details>
<summary>Hint 2</summary>

Pay attention to the operating system age and installed privileged utilities.

</details>

<details>
<summary>Hint 3</summary>

One common route uses a known local privilege escalation vulnerability.

</details>

<details>
<summary>Hint 4</summary>

The vulnerability affects `sudo`.

</details>

<details>
<summary>Hint 5</summary>

The public nickname for the issue is Baron Samedit.

</details>

<details>
<summary>Hint 6</summary>

The CVE is CVE-2021-3156.

</details>

<details>
<summary>Hint 7</summary>

This route is useful, but it is not the route we used.

</details>

---

## Root route B

This is the route we used.

<details>
<summary>Hint 1</summary>

Do not stop at the user shell. Revisit the services you already interacted with.

</details>

<details>
<summary>Hint 2</summary>

Inspect the action-runner service from the target shell.

</details>

<details>
<summary>Hint 3</summary>

Find its configuration file and read the defined actions.

</details>

<details>
<summary>Hint 4</summary>

Check which Linux user is running the action-runner service.

</details>

<details>
<summary>Hint 5</summary>

One action runs a missing script from a root-owned location.

</details>

<details>
<summary>Hint 6</summary>

You cannot create that script directly as the low-privileged user.

</details>

<details>
<summary>Hint 7</summary>

Another action pulls and runs the Ansible playbook from the Git repository.

</details>

<details>
<summary>Hint 8</summary>

The important question is not only “what user does the playbook say to use?”, but “what user launched Ansible?”

</details>

<details>
<summary>Hint 9</summary>

The playbook originally runs tasks as the low-privileged user, which prevents writing to the root-owned location.

</details>

<details>
<summary>Hint 10</summary>

Change the playbook so Ansible does not SSH in as the low-privileged user.

</details>

<details>
<summary>Hint 11</summary>

Force Ansible to run locally.

</details>

<details>
<summary>Hint 12</summary>

If local execution complains about temporary paths, point Ansible's temporary directories somewhere writable.

</details>

<details>
<summary>Hint 13</summary>

Add a harmless check first to confirm the effective user of the local Ansible task.

</details>

<details>
<summary>Hint 14</summary>

Once the local Ansible task runs as root, use the playbook to create the missing backup script.

</details>

<details>
<summary>Hint 15</summary>

Trigger the playbook action first, then trigger the backup action.

</details>

<details>
<summary>Hint 16</summary>

For an interactive root shell, the root-run backup script can create a SUID copy of a shell.

</details>

<details>
<summary>Hint 17</summary>

When using the SUID shell, preserve privileges.

</details>

<details>
<summary>Hint 18</summary>

The final flag is in root's home directory.

</details>

---

## Final comparison hint

<details>
<summary>Hint 1</summary>

There are at least two valid root routes.

</details>

<details>
<summary>Hint 2</summary>

One route uses a known local sudo vulnerability.

</details>

<details>
<summary>Hint 3</summary>

The other route uses the intended application chain: Git-controlled Ansible plus a root-run action service.

</details>

<details>
<summary>Hint 4</summary>

Our route avoided the local sudo exploit and abused the way the services trusted each other.

</details>
