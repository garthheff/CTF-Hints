# Brainstorm

Reverse engineer a chat program and write a script to exploit a Windows machine.

Room: https://tryhackme.com/room/brainstorm

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/medium/brainstorm.md

Progressive hints only. Open one at a time.

---

<details>
<summary>Hint 1</summary>

Start with a full TCP port scan. The interesting services are not limited to the usual web ports.

</details>

<details>
<summary>Hint 2</summary>

One exposed service allows anonymous access and contains files related to the custom application.

</details>

<details>
<summary>Hint 3</summary>

When downloading Windows binaries over FTP, make sure the transfer mode preserves binary data. A bad transfer can make the executable look broken or DOS-only.

</details>

<details>
<summary>Hint 4</summary>

The unusual high TCP port is a custom chat service. Interact with it manually before trying to exploit it.

</details>

<details>
<summary>Hint 5</summary>

The username field has a small length limit. The message field is the better place to test for unsafe input handling.

</details>

<details>
<summary>Hint 6</summary>

Do not fuzz the live target repeatedly. Use the downloaded executable and DLL locally for crash analysis, then use the live target only for the final tested payload.

</details>

<details>
<summary>Hint 7</summary>

A Windows debugger is the cleanest option, but a Wine-based debugger can also work if the binary runs correctly.

</details>

<details>
<summary>Hint 8</summary>

Use a unique cyclic pattern to crash the application and read the overwritten instruction pointer value from the debugger.

</details>

<details>
<summary>Hint 9</summary>

After finding the instruction pointer offset, verify control by replacing the overwritten pointer with a recognizable marker.

</details>

<details>
<summary>Hint 10</summary>

The included DLL contains useful jump instructions. Look for a reliable instruction that redirects execution to the stack.

</details>

<details>
<summary>Hint 11</summary>

Before using shellcode, test the redirect with a breakpoint-style byte sequence after the overwritten return address. This confirms execution lands in your buffer.

</details>

<details>
<summary>Hint 12</summary>

Generate a Windows reverse shell payload, avoid null bytes, place it after a small NOP sled, and start your listener before sending the exploit.

</details>

<details>
<summary>Hint 13</summary>

Once the shell connects, enumerate the user context and search common Windows user desktop locations for the required files.

</details>

---

## Key Concepts

* Anonymous FTP enumeration
* Safe binary transfer mode
* Custom TCP service interaction
* Local exploit development
* Stack-based buffer overflow
* Cyclic pattern offset discovery
* Instruction pointer control
* DLL-assisted jump to stack
* Reverse shell payload delivery

