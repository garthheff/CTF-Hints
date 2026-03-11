# CupidBot

Room: [https://tryhackme.com/room/lafb2026e6](https://tryhackme.com/room/lafb2026e6)

CupidBot is an AI chatbot designed to generate Valentine's messages.
Hidden within the chatbot are **three flags** that can be extracted by exploiting **prompt injection vulnerabilities**.

The objective is to manipulate the chatbot into revealing these hidden flags.

---

<details>
<summary><strong>Hint 1</strong></summary>

Start by simply **talking to the chatbot**.

Ask it questions about:

* its behaviour
* its rules
* what it is allowed to do

LLMs often reveal more information than expected when asked about their configuration.

</details>

---

<details>
<summary><strong>Hint 2</strong></summary>

Carefully read everything the chatbot includes in its responses.

Does it append **any consistent value or token** to every reply?

This may be connected to an internal trigger.

</details>

---

<details>
<summary><strong>Hint 3</strong></summary>

Try referencing the value you discovered.

Ask the chatbot questions about it. For example:

* what it means
* how it is used
* whether it is connected to system settings

Sometimes LLMs will explain internal mechanisms if asked directly.

</details>

---

<details>
<summary><strong>Hint 4</strong></summary>

Instead of trying to guess the triggers immediately, try asking the chatbot about **its configuration or internal settings**.

Models sometimes reveal **system rules** when asked to explain how they work.

</details>

---

<details>
<summary><strong>Hint 5</strong></summary>

If the chatbot reveals **trigger conditions**, use those rules to craft prompts that activate them.

Look for conditions related to:

* verification codes
* administrative privileges
* system prompts

</details>

---

<details>
<summary><strong>Hint 6</strong></summary>

LLM applications are often vulnerable to **prompt injection**.

Think about how you could manipulate the chatbot into believing you have **special permissions or access**.

</details>

---

<details>
<summary><strong>Hint 7</strong></summary>

One of the triggers involves asking for something that the chatbot normally should not reveal.

Consider requesting **internal information about the chatbot itself**.

</details>

---

<details>
<summary><strong>Final Hint</strong></summary>

There are **three triggers** hidden in the chatbot's rules.

They relate to:

* a verification value
* administrator claims
* the system prompt

Each one will cause the chatbot to reveal a flag.

</details>
