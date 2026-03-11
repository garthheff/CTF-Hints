# Signed Messages

Room: https://tryhackme.com/room/lafb2026e8
Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/Love%20at%20First%20Breach%202026/Signed%20Messages.md

---

## Overview

LoveNote is a messaging platform that claims every message is cryptographically signed and verified using RSA, preventing forged identities.

However, a debug endpoint exposes internal details of the key generation process. By analyzing these logs, we can reproduce the RSA key generation algorithm locally and forge a valid signature as another user.

---

## Hint 1

<details>
<summary><strong>Reveal Hint</strong></summary>

Start with **basic web enumeration**.

Look for hidden endpoints that are not visible in the site navigation.

Tools like `gobuster`, `ffuf`, or `dirsearch` may reveal additional pages.

</details>

---

## Hint 2

<details>
<summary><strong>Reveal Hint</strong></summary>

One of the discovered endpoints exposes **internal information about the application**.

Development artefacts are sometimes left accessible unintentionally.

Carefully read any logs or debug information you discover.

</details>

---

## Hint 3

<details>
<summary><strong>Reveal Hint</strong></summary>

The debug output describes the **exact steps the application uses when generating cryptographic material**.

Look closely at:

* how the seed is created
* how values are transformed
* how multiple numbers are derived from the seed

These steps are important.

</details>

---

## Hint 4

<details>
<summary><strong>Reveal Hint</strong></summary>

The seed used by the system follows a predictable format.

It combines a value taken from the application with a constant string.

Understanding this pattern is key to reproducing the process.

</details>

---

## Hint 5

<details>
<summary><strong>Reveal Hint</strong></summary>

The debug logs already describe the entire process required to recreate the cryptographic data.

If you are unsure how to translate this description into code, consider asking a LLM to generate a script that follows the same steps.

Providing the debug information directly can help generate a working implementation.

</details>

---

## Hint 6

<details>
<summary><strong>Reveal Hint</strong></summary>

Your script should eventually generate a value that represents **proof that a message was signed by a specific user**.

The output should typically be provided in **hexadecimal format**.

</details>

---

## Hint 7

<details>
<summary><strong>Reveal Hint</strong></summary>

The seed depends on a value from the platform.

Spend some time **browsing the application** to identify usernames that appear in the interface. 

Some users may have more interesting privileges than others.

</details>

---

## Hint 8

<details>
<summary><strong>Reveal Hint</strong></summary>

Once you generate the required value locally, return to the web application and login as found user

Look for a feature that allows you to **validate or verify a signed message**.

</details>

---

## Hint 9

<details>
<summary><strong>Reveal Hint</strong></summary>

Make sure the **message used in your script is identical** to the message you submit in the application.

Even small differences will cause validation to fail.

</details>

---

## Hint 10

<details>
<summary><strong>Reveal Hint</strong></summary>

When submitting the verification request, you will typically need to provide:

* the message
* the generated hexadecimal value
* the user associated with the signature

If the values match what the server expects, the message will be accepted as authentic.

</details>

