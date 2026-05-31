# jump

Room: https://tryhackme.com/room/jump

Walthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/jr-penetration-tester/jump.md

Use privilege escalation knowledge to jump from a normal user to root.

You’ve discovered a misconfigured internal automation pipeline running on a server. The system processes recon scripts, development backups, monitoring jobs, and deployment tasks across multiple users. Each stage of the pipeline relies too heavily on the previous one. By abusing these trust boundaries, you must move laterally through the system.

Your objective is to escalate from anonymous access all the way through:

recon_user → dev_user → monitor_user → ops_user → root

---

## Initial Access

### Goal

Get a shell as `recon_user`.

<details>
<summary>Hint 1</summary>

Anonymous FTP is available. Check which directories are writable.

</details>

<details>
<summary>Hint 2</summary>

There are multiple `incoming` folders. Not all of them are processed by the same automation.

</details>

<details>
<summary>Hint 3</summary>

The useful folder is the root-level FTP `incoming` directory, not the one under `pub`.

</details>

<details>
<summary>Hint 4</summary>

The automation processes script-like files. Think about a small shell script rather than a compiled payload.

</details>

---

## recon_user Flag

### Goal

Find the first user flag.

<details>
<summary>Hint 1</summary>

Once you have the initial shell, check who you are and inspect the user home directories.

</details>

<details>
<summary>Hint 2</summary>

The flag is in the home directory for the user you land as.

</details>

---

## recon_user to dev_user

### Goal

Escalate from `recon_user` to `dev_user`.

<details>
<summary>Hint 1</summary>

Check your group memberships. One of them matters.

</details>

<details>
<summary>Hint 2</summary>

Look under `/opt` for development-related files.

</details>

<details>
<summary>Hint 3</summary>

A backup script is writable because of group permissions.

</details>

<details>
<summary>Hint 4</summary>

The backup script is not useful just because you can run it manually. Think about how the room's automation triggers job types.

</details>

<details>
<summary>Hint 5</summary>

Use the backup automation to make future access as `dev_user` cleaner. SSH keys are more reliable than repeated reverse shells.

</details>

---

## dev_user Flag

### Goal

Read the `dev_user` flag.

<details>
<summary>Hint 1</summary>

After the backup stage runs, confirm you can authenticate as `dev_user`.

</details>

<details>
<summary>Hint 2</summary>

The flag is in `dev_user`'s home directory.

</details>

---

## dev_user to monitor_user

### Goal

Escalate from `dev_user` to `monitor_user`.

<details>
<summary>Hint 1</summary>

Look for systemd services or timers related to monitoring or health checks.

</details>

<details>
<summary>Hint 2</summary>

Inspect the service user and environment.

</details>

<details>
<summary>Hint 3</summary>

The service runs as `monitor_user`, but its `PATH` includes a location controlled by `dev_user`.

</details>

<details>
<summary>Hint 4</summary>

The healthcheck script calls common system commands without absolute paths.

</details>

<details>
<summary>Hint 5</summary>

This is a PATH hijack. Focus on the command that lists processes.

</details>

<details>
<summary>Hint 6</summary>

Do not leave the hijacked command hanging. The original process flow matters for this step.

</details>

<details>
<summary>Hint 7</summary>

A clean approach is to do your action in the background, then let the real command continue.

</details>

---

## Checking for the healthcheck Timer Issue

### Goal

Work out whether the room state has already missed the monitor escalation trigger.

<details>
<summary>Hint 1</summary>

Check the timer and service status before assuming your payload is wrong.

</details>

<details>
<summary>Hint 2</summary>

If the timer shows `active (elapsed)` and `Trigger: n/a`, it may have already fired.

</details>

<details>
<summary>Hint 3</summary>

If the timer list shows no future `NEXT` run, your hijack may never execute in the current state.

</details>

<details>
<summary>Hint 4</summary>

If the service result is success but the timer has no next run, the service likely ran cleanly before your payload was ready.

</details>

<details>
<summary>Hint 5</summary>

If this happens, redeploying or rebooting the room may be needed. When retrying, make sure the hijack is clean and ready as early as possible.

</details>

---

## monitor_user Flag

### Goal

Read the `monitor_user` flag.

<details>
<summary>Hint 1</summary>

After the PATH hijack works, get stable access as `monitor_user`.

</details>

<details>
<summary>Hint 2</summary>

The flag is in the `monitor_user` home directory.

</details>

---

## monitor_user to ops_user

### Goal

Escalate from `monitor_user` to `ops_user`.

<details>
<summary>Hint 1</summary>

Search for files owned by `ops_user`.

</details>

<details>
<summary>Hint 2</summary>

There is a deployment script owned by `ops_user`.

</details>

<details>
<summary>Hint 3</summary>

The deployment script calls a helper from an application directory.

</details>

<details>
<summary>Hint 4</summary>

The helper file is controlled by `monitor_user`.

</details>

<details>
<summary>Hint 5</summary>

Check `sudo -l` as `monitor_user`. There is a very specific allowed command.

</details>

<details>
<summary>Hint 6</summary>

Use the allowed command to run the deployment flow as `ops_user`.

</details>

---

## ops_user Flag

### Goal

Read the `ops_user` flag.

<details>
<summary>Hint 1</summary>

After the deployment helper runs as `ops_user`, get stable access as that user.

</details>

<details>
<summary>Hint 2</summary>

The flag is in the `ops_user` home directory.

</details>

---

## ops_user to root

### Goal

Escalate from `ops_user` to root.

<details>
<summary>Hint 1</summary>

Check `sudo -l`.

</details>

<details>
<summary>Hint 2</summary>

The allowed root command is an interactive pager.

</details>

<details>
<summary>Hint 3</summary>

Some pagers allow shell escape.

</details>

<details>
<summary>Hint 4</summary>

GTFOBins is useful for this final step.

</details>

---

## Root Flag

### Goal

Read the final flag.

<details>
<summary>Hint 1</summary>

Once you have root, check the root user's home directory.

</details>

<details>
<summary>Hint 2</summary>

The flag follows the same naming pattern as the other user flags.

</details>

---

## Overall Lessons

<details>
<summary>Hint 1</summary>

The room is about chained automation trust.

</details>

<details>
<summary>Hint 2</summary>

Each user can influence something that a more privileged user runs.

</details>

<details>
<summary>Hint 3</summary>

The most fragile step is the monitor escalation. If your payload blocks the expected process, the escalation may appear broken.

</details>
