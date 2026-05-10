# JPGChat

Exploiting poorly made custom chatting service written in a certain language...

Room: https://tryhackme.com/room/jpgchat

https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/JPGChat.md

---

## Hint 1

<details>
<summary>Hint</summary>

Start with normal service enumeration. One of the services may not be identified correctly by the scanner, but the banner gives you enough information to work with.

</details>

---

## Hint 2

<details>
<summary>Hint</summary>

Do not trust the service name guessed by the scanner too much. Read the text returned by the service itself.

</details>

---

## Hint 3

<details>
<summary>Hint</summary>

The custom service tells you where to look next. Pay close attention to the line about the source code.

</details>

---

## Hint 4

<details>
<summary>Hint</summary>

This room expects a small OSINT step. Try to find the public source code for the chat service.

</details>

---

## Hint 5

<details>
<summary>Hint</summary>

Once you find the source code, focus on how the report feature handles user input.

</details>

---

## Hint 6

<details>
<summary>Hint</summary>

Look for places where user input is passed into a shell command. Input that reaches a shell without sanitisation is often dangerous.

</details>

---

## Hint 7

<details>
<summary>Hint</summary>

The report form asks for a name and a report. Both are interesting because both are written into a file using shell commands.

</details>

---

## Hint 8

<details>
<summary>Hint</summary>

Think about how to break out of the intended echo command and make the server execute something else.

</details>

---

## Hint 9

<details>
<summary>Hint</summary>

A simple command execution test may not show output through the service. If it behaves blind, use a callback technique to confirm code execution.

</details>

---

## Hint 10

<details>
<summary>Hint</summary>

For the initial shell, a named pipe reverse shell works well through the vulnerable report input.

</details>

---

## Hint 11

<details>
<summary>Hint</summary>

After getting a shell, check who you are and look around the user home directory.

</details>

---

## Hint 12

<details>
<summary>Hint</summary>

The user flag is available from the low privileged shell. No extra escalation is needed for that part.

</details>

---

## Hint 13

<details>
<summary>Hint</summary>

For privilege escalation, check what the current user can run with sudo.

</details>

---

## Hint 14

<details>
<summary>Hint</summary>

The sudo permissions include a Python script. The important detail is not only the script path, but also the environment variable that sudo preserves.

</details>

---

## Hint 15

<details>
<summary>Hint</summary>

If sudo preserves PYTHONPATH, Python import paths become important. Review the script and look for imported modules.

</details>

---

## Hint 16

<details>
<summary>Hint</summary>

The root-run script imports a module called compare. If you can control where Python looks first, you can supply your own module.

</details>

---

## Hint 17

<details>
<summary>Hint</summary>

Create a fake module with the same name as the imported module. Make it provide the expected object or function so the script does not crash immediately.

</details>

---

## Hint 18

<details>
<summary>Hint</summary>

The malicious module should define the same structure the script expects, but perform a privileged action when called.

</details>

---

## Hint 19

<details>
<summary>Hint</summary>

Run the allowed sudo Python command while pointing PYTHONPATH at the directory containing your fake module.

</details>

---

## Hint 20

<details>
<summary>Hint</summary>

Once the Python module hijack works, you should have a root shell. The root flag is in the usual location.

</details>

---

# Concept Recap

<details>
<summary>Initial Access Concept</summary>

The service exposes source code publicly. Reviewing the source reveals command injection because user controlled input is passed directly into shell commands.

</details>

<details>
<summary>Privilege Escalation Concept</summary>

The low privileged user can run a Python script as root with PYTHONPATH preserved. This allows Python module hijacking by placing a malicious module earlier in the import path.

</details>
