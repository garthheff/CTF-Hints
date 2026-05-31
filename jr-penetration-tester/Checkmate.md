# Checkmate

Room: https://tryhackme.com/room/checkmate

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/Checkmate.md

Exploit weak password practices across Marco’s internal systems to achieve full compromise.

# Hints

## General Approach

<details>
<summary>Hint 1</summary>

Treat each web service as a small password-pattern lesson rather than one big exploit chain.

</details>

<details>
<summary>Hint 2</summary>

Read the wording of each task carefully. The challenge descriptions usually tell you what kind of password weakness to test.

</details>

<details>
<summary>Hint 3</summary>

Useful tools for this room include Hydra, CeWL, CUPP, and a hash lookup or hashing tool.

</details>

---

## Service 1

<details>
<summary>Hint 1</summary>

The first service is a management console. Think about what kind of credentials are often left unchanged on appliances.

</details>

<details>
<summary>Hint 2</summary>

Do not start with huge brute-force lists. A short default credential list is more appropriate.

</details>

<details>
<summary>Hint 3</summary>

Check the failed login message and use it as the failure condition in your login testing tool.

</details>

<details>
<summary>Hint 4</summary>

If a default-credential wordlist does not work, check whether the file format is actually suitable as a plain password list.

</details>

---

## Service 2

<details>
<summary>Hint 1</summary>

The clue points at company language and wording on the page.

</details>

<details>
<summary>Hint 2</summary>

A website wordlist generator is useful here.

</details>

<details>
<summary>Hint 3</summary>

If you know the password length, filter the generated words to that exact length.

</details>

<details>
<summary>Hint 4</summary>

Do not overcomplicate this one. The useful candidate list should be small enough to inspect manually.

</details>

---

## Service 3

<details>
<summary>Hint 1</summary>

The page exposes personal information about a specific employee.

</details>

<details>
<summary>Hint 2</summary>

Names, nicknames, surnames, and dates of birth are all useful when building a targeted password list.

</details>

<details>
<summary>Hint 3</summary>

There is a tool designed specifically for generating password candidates from personal information.

</details>

<details>
<summary>Hint 4</summary>

Try both the real first name and the nickname as possible usernames if needed.

</details>

---

## Image Filename Task

<details>
<summary>Hint 1</summary>

The long filename is a hash, but pay close attention to what was hashed.

</details>

<details>
<summary>Hint 2</summary>

The task says the original filename was hashed, not the file contents.

</details>

<details>
<summary>Hint 3</summary>

Try looking for words or encoded strings around the profile, page content, or extracted data.

</details>

<details>
<summary>Hint 4</summary>

If you find a likely word, hash only the word itself first. Do not automatically add an extension unless the challenge says the extension was part of the original filename.

</details>

<details>
<summary>Hint 5</summary>

An online hash lookup can also help if the original filename is a common word.

</details>

---

## Final Access

<details>
<summary>Hint 1</summary>

The final password is not random. It follows a pattern revealed by the user.

</details>

<details>
<summary>Hint 2</summary>

Break the pattern into parts: keyword, number or year, and punctuation.

</details>

<details>
<summary>Hint 3</summary>

If you know the total password length, use that to work out the required keyword length.

</details>

<details>
<summary>Hint 4</summary>

Generate keywords from the relevant company page instead of guessing from a generic list.

</details>

<details>
<summary>Hint 5</summary>

Use a small script to apply the pattern to each keyword, then test the generated list against SSH.

</details>

<details>
<summary>Hint 6</summary>

Try common year values first, especially the year mentioned by the password-pattern clue.

</details>

---

## Tool Reminders

<details>
<summary>Hint 1</summary>

Hydra can test web forms when you know the username field, password field, and failed-login text.

</details>

<details>
<summary>Hint 2</summary>

CeWL can create a wordlist from words visible on a website.

</details>

<details>
<summary>Hint 3</summary>

CUPP can create password candidates from personal details.

</details>

<details>
<summary>Hint 4</summary>

For hashes, make sure you know whether you are hashing text, a filename, or a file.

</details>
