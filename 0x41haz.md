# 0x41haz

Simple Reversing Challenge

In this challenge, you are asked to solve a simple reversing solution. Download and analyze the binary to discover the password.

There may be anti-reversing measures in place!

Room: https://tryhackme.com/room/0x41haz

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/0x41haz.md

-----------------

# 0x41haz Hints

## Method 1: Repairing the ELF

### Hint 1

<details>
<summary>Hint 1</summary>

Start by checking what type of file you are dealing with.

```bash
file 0x41haz-1640335532346.0x41haz
```

Pay close attention to anything unusual in the output.

</details>

### Hint 2

<details>
<summary>Hint 2</summary>

The binary is an ELF file, but the header has been tampered with.

If `file` reports something like:

```text
ELF 64-bit MSB unknown arch 0x3e00
```

that is a strong clue that the file is being interpreted with the wrong endian setting.

</details>

### Hint 3

<details>
<summary>Hint 3</summary>

Inspect the first few bytes of the ELF header.

```bash
xxd -g1 -l 32 0x41haz-1640335532346.0x41haz
```

The ELF magic should begin with:

```text
7f 45 4c 46
```

The byte at offset `0x05` controls endianess.

</details>

### Hint 4

<details>
<summary>Hint 4</summary>

For a normal x86-64 Linux ELF, the endian byte should be little endian.

In this challenge, the header likely has:

```text
7f 45 4c 46 02 02 01
```

The second `02` means big endian.

For x86-64, it should be:

```text
7f 45 4c 46 02 01 01
```

</details>

### Hint 5

<details>
<summary>Hint 5</summary>

Patch only the endian byte and then re-check the file.

```bash
cp 0x41haz-1640335532346.0x41haz fixed
printf '\x01' | dd of=fixed bs=1 seek=5 count=1 conv=notrunc
file fixed
```

After patching, it should be detected as an x86-64 PIE executable.

</details>

### Hint 6

<details>
<summary>Hint 6</summary>

Run the repaired binary.

```bash
chmod +x fixed
./fixed
```

It should now behave like a normal crackme and ask for a password.

</details>

---

## Method 2: Quick Dynamic Check

### Hint 1

<details>
<summary>Hint 1</summary>

Once the binary runs, trace its library calls.

```bash
ltrace ./fixed
```

Enter a test password such as:

```text
aaaa
```

</details>

### Hint 2

<details>
<summary>Hint 2</summary>

Look for common password comparison functions such as:

```text
strcmp
strncmp
memcmp
```

If you do not see them, the program is probably checking the password manually.

</details>

### Hint 3

<details>
<summary>Hint 3</summary>

If `ltrace` only shows calls like:

```text
gets
strlen
puts
exit
```

then focus on the code around the `strlen` call.

The length check usually happens close to the actual password comparison logic.

</details>

---

## Method 3: Static Analysis With objdump

### Hint 1

<details>
<summary>Hint 1</summary>

Disassemble the repaired binary.

```bash
objdump -d -M intel fixed > disasm.txt
```

</details>

### Hint 2

<details>
<summary>Hint 2</summary>

Search for the `strlen` call.

```bash
grep -n "strlen" disasm.txt
```

Then inspect the nearby code.

```bash
sed -n '105,160p' disasm.txt
```

</details>

### Hint 3

<details>
<summary>Hint 3</summary>

Look for the password length check.

A comparison like this means the required input length is 13 characters:

```asm
cmp DWORD PTR [rbp-0x8],0xd
```

</details>

### Hint 4

<details>
<summary>Hint 4</summary>

Before user input is checked, the binary stores the expected password on the stack.

Look for instructions like:

```asm
movabs rax,0x6667243532404032
mov    QWORD PTR [rbp-0x16],rax
mov    DWORD PTR [rbp-0xe],0x40265473
mov    WORD PTR [rbp-0xa],0x4c
```

Those values are the password bytes.

</details>

### Hint 5

<details>
<summary>Hint 5</summary>

Because x86-64 is little endian, the bytes are stored in reverse order.

For example:

```text
0x6667243532404032
```

becomes:

```text
32 40 40 32 35 24 67 66
```

Convert those bytes to ASCII.

</details>

### Hint 6

<details>
<summary>Hint 6</summary>

The compare loop checks two stack locations byte by byte:

```text
correct password: rbp-0x16
your input:        rbp-0x40
```

If every byte matches, the program prints:

```text
Well Done !!
```

</details>

---

## Method 4: Solving With GDB

### Hint 1

<details>
<summary>Hint 1</summary>

GDB can reveal the password directly from the stack.

Start GDB:

```bash
gdb ./fixed
```

Then set Intel syntax:

```gdb
set disassembly-flavor intel
```

</details>

### Hint 2

<details>
<summary>Hint 2</summary>

The binary is PIE, so the addresses from `objdump` are offsets, not final runtime addresses.

Start the program at the first instruction:

```gdb
starti
```

Then check the memory mappings:

```gdb
info proc mappings
```

Find the base address for the `fixed` binary.

</details>

### Hint 3

<details>
<summary>Hint 3</summary>

Add the `objdump` offset to the PIE base address.

Example:

```text
base address: 0x555555554000
objdump offset: 0x1188
runtime address: 0x555555555188
```

</details>

### Hint 4

<details>
<summary>Hint 4</summary>

Break after the password setup instructions have executed.

Example:

```gdb
break *0x555555555188
continue
```

Do not try to run assembly instructions like `movabs` manually in GDB. Those are program instructions, not GDB commands.

</details>

### Hint 5

<details>
<summary>Hint 5</summary>

Once stopped after the password has been written to the stack, inspect the stored string.

```gdb
x/s $rbp-0x16
```

This reveals the password directly.

</details>

---

## Final Hint

<details>
<summary>Final Hint</summary>

The password is 13 characters long.

It starts with:

```text
2@@25
```

You can recover the rest by either:

1. Decoding the little endian stack values from `objdump`
2. Breaking after the stack writes in GDB and running:

```gdb
x/s $rbp-0x16
```

</details>

---

## Spoiler

<details>
<summary>Spoiler</summary>

Patch the ELF header first:

```bash
cp 0x41haz-1640335532346.0x41haz fixed
printf '\x01' | dd of=fixed bs=1 seek=5 count=1 conv=notrunc
chmod +x fixed
```

Then recover the password from the stack.

Masked password:

```text
2@@25...&@L
```

When entered, the program prints:

```text
Well Done !!
```

</details>
