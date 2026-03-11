# Cupid's Matchmaker

Room: https://tryhackme.com/room/lafb2026e3

Walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/Love%20at%20First%20Breach%202026/Cupid's%20Matchmaker.md

**Cupid's Matchmaker** claims that real humans manually review each personality survey to match users with compatible singles.

That wording is a strong clue. If staff are reviewing submitted responses in an admin panel, any unsanitized input may later be rendered in a privileged user’s browser. That makes **stored XSS** a likely attack path.

If you are stuck, expand the hints below gradually. Try not to skip ahead unless you really need to.

---

<details>
<summary><strong>Hint 1</strong></summary>

Look carefully at the **language used in the application description**.

Is the matching process automated, or does it involve people reviewing submissions?

Think about what happens when **other users or staff view data you submit**.

</details>

---

<details>
<summary><strong>Bigger Hint 1</strong></summary>

If another user or administrator views your submitted content in their browser, any unsanitized input might execute when it is rendered.

What class of web vulnerability involves **injecting JavaScript that runs when someone else loads a page?**

</details>

---

<details>
<summary><strong>Hint 2</strong></summary>

Try submitting **JavaScript** in one of the survey fields.

</details>

---

<details>
<summary><strong>Bigger Hint 2</strong></summary>

If your JavaScript executes in another user’s browser, what useful information might you be able to access?

JavaScript running in the page can access certain browser data through the `document` object.

</details>

---

<details>
<summary><strong>Hint 3</strong></summary>

If you want to verify that your payload executes, you can make the browser send a request to a server you control.

A very simple way to capture incoming requests is to run a small local web server.

</details>

---

<details>
<summary><strong>Bigger Hint 3</strong></summary>

Python has a built-in module that can quickly start a basic HTTP server.

This allows you to capture requests sent by a victim browser. Can also use curl.

</details>

---

<details>
<summary><strong>Hint 4</strong></summary>

Once your server is running, you need a payload that **forces the browser to make a request to your machine**.

There are several ways to do this in JavaScript, including using `fetch()` or creating a new image element.

</details>

---

<details>
<summary><strong>Bigger Hint 4</strong></summary>

You can send useful data as part of the URL.

For example, JavaScript can read cookies with:

```
document.cookie
```

Encoding the value before sending it helps ensure it is transmitted safely in a URL.

</details>

---

<details>
<summary><strong>Final Hint</strong></summary>

Combine the following ideas:

* Inject JavaScript into a survey field
* Have that JavaScript send a request to a server you control
* Include the cookie value in the request

When the survey is reviewed, your server should receive a request containing the flag.

</details>

