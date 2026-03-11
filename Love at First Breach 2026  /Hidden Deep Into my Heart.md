# Deep Into my Heart

Room: https://tryhackme.com/room/lafb2026e9

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/Love%20at%20First%20Breach%202026/Hidden%20Deep%20Into%20my%20Heart.md

## Scenario

Cupid's Vault was designed to protect secrets meant to stay hidden forever. Intelligence suggests Cupid may have unintentionally left vulnerabilities in the system.

Your task is to investigate the web application and retrieve the hidden flag.

---

## Stage 1

<details>
<summary><strong>Hint 1</strong></summary>

Start with basic reconnaissance.

Check common files that sometimes reveal hidden paths or information.

</details>

<details>
<summary><strong>Hint 2</strong></summary>

Look for a file used by search engines that tells them what they should avoid crawling.

</details>

<details>
<summary><strong>Hint 3</strong></summary>

Inside this file you may find:

* A hidden directory
* A suspicious comment that may contain useful information

</details>

---

## Stage 2

<details>
<summary><strong>Hint 1</strong></summary>

You discovered a directory in the previous step.

Try enumerating files and folders inside it.

</details>

<details>
<summary><strong>Hint 2</strong></summary>

Directory brute forcing tools can help.

Example tool:

```
gobuster
```

</details>

<details>
<summary><strong>Hint 3</strong></summary>

Use a common wordlist to search the discovered directory.

Example:

```
gobuster dir -u http://TARGET/cupids_secret_vault -w /usr/share/wordlists/dirb/common.txt
```

</details>

---

## Stage 3 

<details>
<summary><strong>Hint 1</strong></summary>

Enumeration should reveal an administrator page.

Try visiting it in your browser.

</details>

<details>
<summary><strong>Hint 2</strong></summary>

Think about the information you found earlier in the `robots.txt` file.

Was there anything that looked like a password?

</details>

<details>
<summary><strong>Hint 3</strong></summary>

Try using the discovered string as the **password**.

Now think about what the **username** for an administrator account might be.

</details>

<details>
<summary><strong>Hint 4</strong></summary>
Maybe wrong password is used if you have used common usernames for administrators. Use a small user list and bruteforce username if needed, but is a very common username for an administrator account. 
</details>

---

## Stage 4

<details>
<summary><strong>Hint</strong></summary>

Once logged in successfully, the flag should be visible within the admin panel.

</details>

