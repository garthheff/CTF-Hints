# Easy Peasy

Practice using tools such as Nmap and GoBuster to locate a hidden directory to get initial access to a vulnerable machine. Then escalate your privileges through a vulnerable cronjob.

Room: https://tryhackme.com/room/easypeasyctf

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/easypeasyctf.md

---

# Easy Peasy Hints

These hints are designed to help you move forward without immediately giving the answer away. Expand one hint at a time.

---

## How many ports are open?

<details>
<summary>Hint 1</summary>

Start with a full TCP port scan instead of only the default top ports.

</details>

<details>
<summary>Hint 2</summary>

Use Nmap against all 65535 TCP ports. Service detection will help later too.

</details>

<details>
<summary>Hint 3</summary>

A command like this is a good start:

```bash
nmap -p- -sV -sC TARGET_IP
```

</details>

<details>
<summary>Hint 4</summary>

You should find three open ports.

</details>

---

## What is the version of nginx?

<details>
<summary>Hint 1</summary>

This comes directly from your Nmap service detection output.

</details>

<details>
<summary>Hint 2</summary>

Look at the service banner on port 80.

</details>

<details>
<summary>Hint 3</summary>

The HTTP server header reveals the nginx version.

</details>

---

## What is running on the highest port?

<details>
<summary>Hint 1</summary>

Compare the open ports from your scan and identify the largest port number.

</details>

<details>
<summary>Hint 2</summary>

There are two web servers on this box. The highest port is another web service.

</details>

<details>
<summary>Hint 3</summary>

Nmap should identify it as Apache HTTPD.

</details>

---

## Using GoBuster, find flag 1.

<details>
<summary>Hint 1</summary>

Start enumerating the web content on port 80.

</details>

<details>
<summary>Hint 2</summary>

Use a common wordlist first. The path you need is not very deep, but you will need to keep enumerating after the first hit.

</details>

<details>
<summary>Hint 3</summary>

A workflow like this helps:

```bash
gobuster dir -u http://TARGET_IP -w /usr/share/wordlists/dirb/common.txt
```

Then run Gobuster again on any interesting directory you discover.

</details>

<details>
<summary>Hint 4</summary>

You should eventually reach a page called `dead end`.

</details>

<details>
<summary>Hint 5</summary>

Check the page source. The flag is hidden and encoded.

</details>

<details>
<summary>Hint 6</summary>

The hidden value is Base64.

</details>

---

## Further enumerate the machine, what is flag 2?

<details>
<summary>Hint 1</summary>

The first web server is not the only one worth investigating.

</details>

<details>
<summary>Hint 2</summary>

Go back to your Nmap results and enumerate the other HTTP service running on the high port.

</details>

<details>
<summary>Hint 3</summary>

Gobuster can still be useful here, but do not ignore `robots.txt`.

</details>

<details>
<summary>Hint 4</summary>

The flag is connected to a value that looks like a hash.

</details>

<details>
<summary>Hint 5</summary>

You can crack that value with an online hash lookup or another lookup method.

</details>

---

## Crack the hash with easypeasy.txt, What is the flag 3?

<details>
<summary>Hint 1</summary>

Do not leave the Apache page too quickly. There is another flag sitting in plain sight if you read carefully.

</details>

<details>
<summary>Hint 2</summary>

You do not actually need the image for this flag. Just inspect the page content.

</details>

<details>
<summary>Hint 3</summary>

The third flag is directly visible in the page response on the Apache service.

</details>

---

## What is the hidden directory?

<details>
<summary>Hint 1</summary>

The Apache page source contains a hidden string and tells you roughly how it is encoded.

</details>

<details>
<summary>Hint 2</summary>

Look for a hidden paragraph in the HTML source.

</details>

<details>
<summary>Hint 3</summary>

The value is:

```text
ObsJmP173N2X6dOrAgEAL0Vu
```

</details>

<details>
<summary>Hint 4</summary>

This is Base62, not Base64.

</details>

<details>
<summary>Hint 5</summary>

Decoding it gives you the hidden directory path on the Apache web server.

</details>

---

## Using the wordlist that provided to you in this task crack the hash what is the password?

<details>
<summary>Hint 1</summary>

Visit the hidden directory you just discovered.

</details>

<details>
<summary>Hint 2</summary>

You will find an image and a long hash value on that page.

</details>

<details>
<summary>Hint 3</summary>

Save the hash to a file and use John with the provided wordlist.

</details>

<details>
<summary>Hint 4</summary>

John may guess the wrong format automatically. Pay attention to its warnings.

</details>

<details>
<summary>Hint 5</summary>

Even if John loads it as something unexpected, the cracked password is still what you need for the next step.

</details>

<details>
<summary>Hint 6</summary>

The cracked password is used as the passphrase for extracting hidden data from the image.

</details>

---

## What is the user flag?

<details>
<summary>Hint 1</summary>

The image is not just decoration. There is hidden data inside it.

</details>

<details>
<summary>Hint 2</summary>

Use a steganography extraction tool against the image.

</details>

<details>
<summary>Hint 3</summary>

A command like this is the right idea:

```bash
steghide extract -sf binarycodepixabay.jpg
```

</details>

<details>
<summary>Hint 4</summary>

Use the password you cracked from the hash as the passphrase.

</details>

<details>
<summary>Hint 5</summary>

The extracted text gives you a username and a binary-encoded password.

</details>

<details>
<summary>Hint 6</summary>

Convert the binary string into text, then SSH to the host on the non-standard SSH port.

</details>

<details>
<summary>Hint 7</summary>

After logging in, read `user.txt`. The flag looks wrong because it is encoded again.

</details>

<details>
<summary>Hint 8</summary>

Use ROT13 on the user flag.

</details>

---

## What is the root flag?

<details>
<summary>Hint 1</summary>

The intended privilege escalation path is not a kernel exploit or SUID trick.

</details>

<details>
<summary>Hint 2</summary>

Check scheduled tasks and system-wide cron configuration.

</details>

<details>
<summary>Hint 3</summary>

Read `/etc/crontab`.

</details>

<details>
<summary>Hint 4</summary>

There is a script in `/var/www/` being run as root every minute.

</details>

<details>
<summary>Hint 5</summary>

Check the ownership and permissions of that script.

</details>

<details>
<summary>Hint 6</summary>

If you can write to the script and root executes it on a schedule, you control code execution as root.

</details>

<details>
<summary>Hint 7</summary>

A reverse shell payload is enough here. Start a listener, overwrite the script, and wait for cron to trigger.

</details>

<details>
<summary>Hint 8</summary>

Once the shell lands, check the root home directory for the root flag file.

</details>

---

## Bonus hint: non-intended privilege escalation path

<details>
<summary>Hint 1</summary>

If you enumerate SUID binaries, one of them stands out.

</details>

<details>
<summary>Hint 2</summary>

`pkexec` is present and vulnerable on this box.

</details>

<details>
<summary>Hint 3</summary>

That route works, but it is not the intended path for the room.

</details>
