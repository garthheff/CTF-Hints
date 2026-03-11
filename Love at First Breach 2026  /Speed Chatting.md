## Speed Chat

**Room:** Speed Chat

---

### Scenario

Days before Valentine’s Day, TryHeartMe rushed out a new messaging platform called **Speed Chatter**, promising instant connections and private conversations. In the rush to launch, security appears to have been ignored.

Our goal was to identify the weakness in the application, gain code execution, and retrieve the flag.

### Hints
---
If you are stuck, expand the hints below **one at a time**.  
Try not to skip ahead unless you really need to.

---

<details>
<summary><strong>Hint 1</strong></summary>

Start by interacting with all available features of the website.

Not every visible feature will be exploitable, but testing them can help identify where the real attack surface might be.

Consider areas where **user-controlled input or files** are accepted.

</details>

---

<details>
<summary><strong>Bigger Hint 1</strong></summary>

One feature allows you to **upload something to the server**.

Uploads are often risky when applications do not properly validate file types or content.

Think about what might happen if the server **processes or executes uploaded files**.

</details>

---

<details>
<summary><strong>Hint 2</strong></summary>

Look at how the application is running.

The service is hosted on **port 5000**, which can sometimes hint at the type of backend framework being used.

If you know what technology might be running behind the site, consider what type of payloads might be compatible with it.

</details>

---

<details>
<summary><strong>Bigger Hint 2</strong></summary>

If the server is using a Python web framework, it may be possible to upload a **Python script** that executes when processed by the application.

Instead of trying traditional web shells, think about how to create a **Python-based reverse connection**.

</details>

---

<details>
<summary><strong>Hint 3</strong></summary>

Once your payload executes, you may receive a connection back to your machine.

You will need a **listener** running to catch it.

</details>

---

<details>
<summary><strong>Bigger Hint 3</strong></summary>

If your connection appears briefly and then dies, think about how web servers handle requests.

A process started inside a request may terminate when the request finishes.

Consider launching your payload in a **separate process** so it continues running after the request completes.

</details>

---

<details>
<summary><strong>Final Hint</strong></summary>

After gaining command execution on the server, look around the current directory.

The flag is accessible from the shell once you have code execution.

</details>

