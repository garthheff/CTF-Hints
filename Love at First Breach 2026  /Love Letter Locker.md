# Love Letter Locker

Room: [https://tryhackme.com/room/lafb2026e2](https://tryhackme.com/room/lafb2026e2)

## Scenario

**Love Letter Locker** is a web app that lets users write and store Valentine’s letters.

From the challenge description alone, the phrase _“For your eyes only?”_ strongly hints that the vulnerability may involve **access control**, which makes **IDOR** a good thing to test early.

---

<details>
<summary><strong>Hint 1</strong></summary>

How does the website identify which message you are viewing?

</details>

---

<details>
<summary><strong>Hint 2</strong></summary>

Look closely at the **letter URL** when viewing a message.

</details>

---

<details>
<summary><strong>Hint 3</strong></summary>

Try changing the number in the URL.  
This is an **IDOR** vulnerability.

</details>
