# smaggrotto — Hints

Room: https://tryhackme.com/room/smaggrotto

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/smaggrotto.md

---

# Smag Grotto Hints

These hints are designed for gradual help. Open one section at a time.

## Hint 1

<details>
<summary>Hint</summary>

Start with web enumeration. The first useful discovery is not hidden very deep, but it is not on the homepage either.

</details>

## Hint 2

<details>
<summary>Hint</summary>

Look for an exposed area that appears to contain old communication or migration-related material.

</details>

## Hint 3

<details>
<summary>Hint</summary>

One of the discovered files is a packet capture. Do not just open it visually. Inspect the traffic closely and look for web requests.

</details>

## Hint 4

<details>
<summary>Hint</summary>

The capture contains credentials sent in a very weak way. Check HTTP form submissions carefully.

</details>

## Hint 5

<details>
<summary>Hint</summary>

After logging in, look for a way to run something on the target rather than just browsing pages.

</details>

## Hint 6

<details>
<summary>Hint</summary>

Once you have a low-privilege shell, upgrade it enough to make enumeration easier.

</details>

## Hint 7

<details>
<summary>Hint</summary>

The next step is not directly reading the user's SSH directory. Check for backup locations and automated jobs.

</details>

## Hint 8

<details>
<summary>Hint</summary>

A root-owned scheduled task copies a backup key into a user's SSH configuration regularly.

</details>

## Hint 9

<details>
<summary>Hint</summary>

The weakness is not the public key itself. The weakness is whether you can change the source file used by the scheduled task.

</details>

## Hint 10

<details>
<summary>Hint</summary>

Place a public key you control where the scheduled task reads from, then wait for the task to run.

</details>

## Hint 11

<details>
<summary>Hint</summary>

After becoming the normal user, check allowed sudo commands before looking for kernel exploits or noisy escalation paths.

</details>

## Hint 12

<details>
<summary>Hint</summary>

The permitted sudo binary has a known GTFOBins-style method to spawn a root shell.

</details>
