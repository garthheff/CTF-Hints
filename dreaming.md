# Dreaming

Solve the riddle that dreams have woven.

While the king of dreams was imprisoned, his home fell into ruins.

Can you help Sandman restore his kingdom?

room: https://tryhackme.com/room/dreaming

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/dreaming.md

## Initial Access

<details>
<summary><strong>Hint 1</strong></summary>

The first scan may not show the full picture. Try again with a less aggressive approach.

</details>

<details>
<summary><strong>Hint 2</strong></summary>

Enumerate the web server for hidden directories.

</details>

<details>
<summary><strong>Hint 3</strong></summary>

The application reveals its exact CMS version.

</details>

<details>
<summary><strong>Hint 4</strong></summary>

The administrator login uses only a password. Start with very common choices.

</details>

<details>
<summary><strong>Hint 5</strong></summary>

Once authenticated, search for a public exploit affecting that exact version.

</details>

## First User

<details>
<summary><strong>Hint 6</strong></summary>

Search for readable files owned by the next user.

</details>

<details>
<summary><strong>Hint 7</strong></summary>

A Python script outside the user’s home contains something useful.

</details>

<details>
<summary><strong>Hint 8</strong></summary>

Consider password reuse.

</details>

## Second User

<details>
<summary><strong>Hint 9</strong></summary>

Check shell history and sudo permissions.

</details>

<details>
<summary><strong>Hint 10</strong></summary>

Compare the output of the permitted script with data stored in MySQL.

</details>

<details>
<summary><strong>Hint 11</strong></summary>

Can you control data that is later passed into a shell command?

</details>

## Final User

<details>
<summary><strong>Hint 12</strong></summary>

Search for readable scripts owned by the next user.

</details>

<details>
<summary><strong>Hint 13</strong></summary>

Pay attention to imported Python modules and their permissions.

</details>

<details>
<summary><strong>Final Hint</strong></summary>

A writable standard-library module can execute your code when the higher-privileged script imports it.

</details>
