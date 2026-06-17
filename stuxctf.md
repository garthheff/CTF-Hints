# StuxCTF

Crypto, serealization, priv scalation and more ..! 

What is the hidden directory?

HINT: g ^ a mod p, g ^ b mod p, g ^ C mod p

first 128 characters  

Room: https://tryhackme.com/room/stuxctf

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/medium/stuxctf.md

-----------------------------

# StuxCTF Hints

⚠️ **STOP**

These hints become progressively more revealing.

Open only as many as you need.

---

## Initial Enumeration

<details>
<summary>Hint 1</summary>

Start with the web service and inspect more than just what is visibly rendered in the browser.

</details>

<details>
<summary>Hint 2</summary>

Check the page source for HTML comments.

</details>

<details>
<summary>Hint 3</summary>

Also check the standard file used to give instructions to search-engine crawlers.

</details>

---

## Finding the Hidden Directory

<details>
<summary>Hint 1</summary>

The large values in the HTML comment are not random.

They relate to modular exponentiation and Diffie-Hellman.

</details>

<details>
<summary>Hint 2</summary>

You are given:

```text
p
g
a
b
g^c
```

The supplied `g^c` value needs to be combined with both `a` and `b`.

</details>

<details>
<summary>Hint 3</summary>

Think about:

```text
(g^c)^(a × b) mod p
```

</details>

<details>
<summary>Hint 4</summary>

Python's built-in `pow()` supports modular exponentiation:

```python
pow(value, exponent, modulus)
```

</details>

<details>
<summary>Hint 5</summary>

The whole calculated value is not the directory name.

Pay attention to the hint mentioning the first 128 characters.

</details>

<details>
<summary>Hint 6</summary>

Try the calculated directory directly beneath the web root.

Do not assume the name found in `robots.txt` must be part of the path.

</details>

---

## Hidden Web Application

<details>
<summary>Hint 1</summary>

Inspect the source of the newly discovered page.

</details>

<details>
<summary>Hint 2</summary>

There is another HTML comment pointing toward a GET parameter.

</details>

<details>
<summary>Hint 3</summary>

The parameter is named:

```text
file
```

</details>

---

## File Parameter

<details>
<summary>Hint 1</summary>

Test whether the parameter can access a known local Linux file.

</details>

<details>
<summary>Hint 2</summary>

Try both absolute paths and directory traversal.

</details>

<details>
<summary>Hint 3</summary>

A useful test file is:

```text
/etc/passwd
```

</details>

---

## Recovering the PHP Source

<details>
<summary>Hint 1</summary>

What happens when you request the application's own PHP filename through the vulnerable parameter?

</details>

<details>
<summary>Hint 2</summary>

The returned data is encoded more than once.

Look closely at the characters used in the output.

</details>

<details>
<summary>Hint 3</summary>

The outer layer is hexadecimal.

</details>

<details>
<summary>Hint 4</summary>

After converting the hexadecimal back into bytes, the result still needs to be reversed.

</details>

<details>
<summary>Hint 5</summary>

The reversed value is Base64.

The approximate decoding order is:

```text
hex decode
→ reverse
→ Base64 decode
```

</details>

---

## Understanding the Vulnerability

<details>
<summary>Hint 1</summary>

Look for dangerous PHP functions operating on content controlled through the `file` parameter.

</details>

<details>
<summary>Hint 2</summary>

Pay particular attention to:

```php
unserialize()
```

</details>

<details>
<summary>Hint 3</summary>

Review the PHP class defined above the vulnerable call.

Its destructor performs a file-writing operation.

</details>

<details>
<summary>Hint 4</summary>

The class contains two properties:

```text
file
data
```

Consider what happens if you control both of them.

</details>

<details>
<summary>Hint 5</summary>

Create a serialized object that writes attacker-controlled PHP code to a file inside the current web directory.

</details>

<details>
<summary>Hint 6</summary>

Generate the serialized object with PHP rather than manually calculating serialized string lengths.

</details>

---

## Delivering the Serialized Object

<details>
<summary>Hint 1</summary>

The application reads the contents of the filename or URL passed through the parameter.

You need to make your serialized payload accessible to the target.

</details>

<details>
<summary>Hint 2</summary>

A temporary Python HTTP server on your AttackBox is sufficient.

</details>

<details>
<summary>Hint 3</summary>

Pass the hosted payload URL to the vulnerable `file` parameter.

Watch your HTTP server to confirm that the target downloads it.

</details>

<details>
<summary>Hint 4</summary>

If the target downloads the payload but no file appears, verify the serialized string lengths.

A one-byte mismatch causes `unserialize()` to fail.

</details>

---

## Initial Access

<details>
<summary>Hint 1</summary>

After successful deserialization, check whether the filename specified in your object is accessible through the browser.

</details>

<details>
<summary>Hint 2</summary>

Test the PHP file with a harmless command such as:

```text
id
```

</details>

<details>
<summary>Hint 3</summary>

Once command execution is confirmed, use it to establish a reverse shell.

</details>

---

## User Flag

<details>
<summary>Hint 1</summary>

Enumerate the directories under:

```text
/home
```

</details>

<details>
<summary>Hint 2</summary>

The user flag is readable by the web-service account.

No lateral movement is required first.

</details>

---

## Privilege Escalation

<details>
<summary>Hint 1</summary>

Begin with the standard checks before searching for complicated kernel or SUID exploits.

</details>

<details>
<summary>Hint 2</summary>

Check the current user's sudo permissions.

</details>

<details>
<summary>Hint 3</summary>

The web-service account can execute commands as any user without supplying a password.

</details>

<details>
<summary>Hint 4</summary>

No GTFOBins technique or binary abuse is required.

Use the sudo permission directly to start a root shell.

</details>

---

## Final Hint

<details>
<summary>Hint 1</summary>

The complete path is:

```text
HTML comments
→ modular exponentiation
→ hidden directory
→ file read
→ encoded PHP source
→ insecure deserialization
→ arbitrary file write
→ web shell
→ reverse shell
→ unrestricted sudo
```

</details>
