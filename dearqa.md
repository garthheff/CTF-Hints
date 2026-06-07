# Dear QA

Are you able to solve this challenge involving reverse engineering and exploit development?

Room: https://tryhackme.com/room/dearqa

## Hints

<details>
<summary>Hint 1</summary>

Start by checking what services are exposed on the target.

</details>

<details>
<summary>Hint 2</summary>

One port is running a custom service that asks for a name.

</details>

<details>
<summary>Hint 3</summary>

The room provides a downloadable binary. Analyse that instead of guessing from the network service alone.

</details>

<details>
<summary>Hint 4</summary>

Look for unsafe input handling and any interesting hidden functions.

</details>

<details>
<summary>Hint 5</summary>

The binary is a ret2win challenge. Work out the offset to the return address and redirect execution to the hidden function.

</details>

<details>
<summary>Final Hint</summary>

The buffer is 32 bytes, and the saved base pointer adds another 8 bytes on x64.

</details>
