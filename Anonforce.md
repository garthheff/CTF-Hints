# BSidesGT: Anonforce — Hints

Room: https://tryhackme.com/room/bsidesgtanonforce

Walkthrough: https://github.com/garthheff/CTF-Writeups/edit/main/TryHackMe/Anonforce.md

---

<details>
<summary>Hint 1 – start with recon</summary>

Run a full port scan.

- What services are exposed?
- Anything that allows file access?

</details>

---

<details>
<summary>Hint 2 – try default access</summary>

One service may allow login without credentials.

- Try `anonymous`
- Does it actually require a password?

</details>

---

<details>
<summary>Hint 3 – check scope of access</summary>

After logging in:

- Are you restricted to one folder?
- Or can you browse the whole system?

</details>

---

<details>
<summary>Hint 4 – find user data</summary>

Look in:

- `/home`
- User directories

You’re looking for flags or clues.

</details>

---

<details>
<summary>Hint 5 – something doesn’t belong</summary>

While exploring `/`:

- Any oddly named directories?
- Anything with very open permissions?

</details>

---

<details>
<summary>Hint 6 – sensitive files</summary>

If you find files:

- Check extensions
- Look for encrypted files or keys

Think:
- `.pgp`
- `.asc`

</details>

---

<details>
<summary>Hint 7 – pairing files</summary>

If you find:
- a key
- an encrypted file

Ask:
- Can one unlock the other?

</details>

---

<details>
<summary>Hint 8 – cracking step</summary>

If the key is protected:

- Can you convert it into a crackable format?
- What tool works with GPG keys?

</details>

---

<details>
<summary>Hint 9 – wordlists</summary>

Once you have a hash:

- Try a common wordlist
- Does it crack quickly?

</details>

---

<details>
<summary>Hint 10 – decryption</summary>

After cracking:

- Import the key
- Decrypt the file

</details>

---

<details>
<summary>Hint 11 – what did you get?</summary>

Look at the decrypted output:

- Does it resemble a system file?
- Any usernames or hashes?

</details>

---

<details>
<summary>Hint 12 – pick the right target</summary>

You may see multiple hashes, try crack both

</details>

---

<details>
<summary>Hint 13 – cracking hashes</summary>

- Use John or Hashcat
- Specify format if needed

</details>

---

<details>
<summary>Hint 14 – access</summary>

Once you have credentials:

- Which service can you use?
- What port was open earlier?

</details>

---

<details>
<summary>Hint 15 – finish</summary>

After access:

- Navigate to root directory
- Retrieve the final flag

</details>

---

## mindset

- Follow what the system gives you  
- Don’t overthink early  
- Chain small wins together  
