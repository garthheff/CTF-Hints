# Peak Hill

Exercises in Python library abuse and some exploitation techniques

Room: [https://tryhackme.com/room/peakhill](https://tryhackme.com/room/peakhill)

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/peakhill.md

---

## Hint 1

<details>
<summary>Hint 1</summary>

Start with a full TCP port scan. One of the open services is standard and allows unauthenticated access.

</details>

---

## Hint 2

<details>
<summary>Hint 2</summary>

Check the file listing carefully on the unauthenticated service. Not every interesting file is always obvious from a casual glance.

</details>

---

## Hint 3

<details>
<summary>Hint 3</summary>

One downloaded file looks like a long stream of binary digits. Treat it as encoded data rather than as a password directly.

</details>

---

## Hint 4

<details>
<summary>Hint 4</summary>

After converting the binary-looking data into bytes, the result resembles a Python serialisation format.

</details>

---

## Hint 5

<details>
<summary>Hint 5</summary>

The decoded data contains fragmented credential-like values. Look for repeated naming patterns and numeric suffixes.

</details>

---

## Hint 6

<details>
<summary>Hint 6</summary>

The fragments need to be reassembled in order. The suffix numbers matter.

</details>

---

## Hint 7

<details>
<summary>Hint 7</summary>

The first recovered credential pair is useful for gaining normal remote access to the machine.

</details>

---

## Hint 8

<details>
<summary>Hint 8</summary>

Once logged in, inspect the home directory. A compiled Python file is the next major clue.

</details>

---

## Hint 9

<details>
<summary>Hint 9</summary>

If the compiled Python file is not recognised normally, inspect its header manually. It may still be a valid Python bytecode file.

</details>

---

## Hint 10

<details>
<summary>Hint 10</summary>

For modern Python bytecode, the actual marshalled code object usually begins after the header rather than at byte zero.

</details>

---

## Hint 11

<details>
<summary>Hint 11</summary>

You do not need the original source file to learn useful things. Python code objects expose names, constants, variable names, and nested functions.

</details>

---

## Hint 12

<details>
<summary>Hint 12</summary>

Look for strings related to login prompts and command execution. This reveals what the mysterious TCP service is doing.

</details>

---

## Hint 13

<details>
<summary>Hint 13</summary>

Two integer constants in the bytecode are not random. The imported Python helper functions reveal how they should be transformed.

</details>

---

## Hint 14

<details>
<summary>Hint 14</summary>

The transformed integer constants provide credentials for the custom TCP service.

</details>

---

## Hint 15

<details>
<summary>Hint 15</summary>

The custom service runs commands as a different local user. Use it to confirm the execution context.

</details>

---

## Hint 16

<details>
<summary>Hint 16</summary>

Commands sent to the custom service do not keep shell state between requests. Directory changes will not persist.

</details>

---

## Hint 17

<details>
<summary>Hint 17</summary>

Use absolute paths or combine directory changes and actions into a single command when interacting with the service.

</details>

---

## Hint 18

<details>
<summary>Hint 18</summary>

The custom service can read the first user flag because it runs as the user who owns it.

</details>

---

## Hint 19

<details>
<summary>Hint 19</summary>

If the custom service is awkward to use, consider creating a temporary helper that lets your existing SSH session execute with the service user’s privileges.

</details>

---

## Hint 20

<details>
<summary>Hint 20</summary>

A copied shell binary can preserve the effective user when the permissions are set correctly. Remember that some shells need a special option to keep elevated privileges.

</details>

---

## Hint 21

<details>
<summary>Hint 21</summary>

After pivoting into the second user, inspect their SSH directory.

</details>

---

## Hint 22

<details>
<summary>Hint 22</summary>

The second user has key-based SSH material available. Move or copy it somewhere your initial SSH user can retrieve it safely.

</details>

---

## Hint 23

<details>
<summary>Hint 23</summary>

Once you can SSH directly as the second user, check what that user can run with elevated privileges.

</details>

---

## Hint 24

<details>
<summary>Hint 24</summary>

The allowed elevated binary cannot be read directly by the user, but it can still be executed. Treat it as a black box.

</details>

---

## Hint 25

<details>
<summary>Hint 25</summary>

Run the elevated binary normally and observe its prompt. Try harmless inputs first.

</details>

---

## Hint 26

<details>
<summary>Hint 26</summary>

Some inputs cause a decoding error. That error message reveals the expected input format.

</details>

---

## Hint 27

<details>
<summary>Hint 27</summary>

The program expects Base64 input, but plain decoded text is not enough.

</details>

---

## Hint 28

<details>
<summary>Hint 28</summary>

The room theme and earlier credential format are important clues. Think about Python deserialisation.

</details>

---

## Hint 29

<details>
<summary>Hint 29</summary>

If attacker-controlled data is decoded and then deserialised using Python’s unsafe object format, it can trigger code execution.

</details>

---

## Hint 30

<details>
<summary>Hint 30</summary>

Create a harmless proof-of-concept payload first, such as one that prints the current execution identity.

</details>

---

## Hint 31

<details>
<summary>Hint 31</summary>

The elevated binary runs the deserialisation as root, so a successful payload executes as root.

</details>

---

## Hint 32

<details>
<summary>Hint 32</summary>

For a stable root shell, use the deserialisation issue to create a temporary root-owned helper with elevated permissions.

</details>

---

## Hint 33

<details>
<summary>Hint 33</summary>

When running the helper shell, preserve privileges. Otherwise the shell may drop the effective root ID.

</details>

---

## Hint 34

<details>
<summary>Hint 34</summary>

The root flag file is intentionally awkward. The name shown by a basic directory listing may not be the exact filename.

</details>

---

## Hint 35

<details>
<summary>Hint 35</summary>

If typing the root flag path directly fails, inspect the filename representation more carefully or use a wildcard pattern.

</details>

---

## Final Nudge

<details>
<summary>Final nudge</summary>

The complete chain is:

FTP encoded data → Python serialisation clue → initial SSH credentials → Python bytecode analysis → custom service credentials → command execution as second user → privilege-preserving pivot → second user SSH key → sudo-only farm binary → Base64 plus unsafe Python deserialisation → root helper shell → wildcard read of oddly named flag file.

</details>
