# TryHeartMe 

Room: https://tryhackme.com/room/lafb2026e5

## Scenario

**My Dearest Hacker,**

The TryHeartMe shop is open for business. Can you find a way to purchase the hidden **Valenflag** item?

---

# Hints

If you get stuck, reveal hints gradually. Each hint expands on the previous one.

---

<details>
<summary><strong>Hint 1</strong></summary>

Look at how the application keeps track of your user information after logging in.

Modern web applications often store authentication data on the client side in cookies.

Check the browser developer tools and inspect the cookies set by the application.

</details>

---

<details>
<summary><strong>Need a bit more?</strong></summary>

One of the cookies contains structured data that can be decoded.

The value is made up of **three sections separated by dots**. This is a common format used for authentication tokens.

</details>

---

<details>
<summary><strong>Hint 2</strong></summary>

Try decoding the token you found.

There are online tools that can decode this format and show you the JSON data stored inside.

Look carefully at the fields in the decoded payload.

</details>

---

<details>
<summary><strong>Need a bigger hint?</strong></summary>

Ask yourself:

Should a user be able to control things like **roles or credits**?

If these values are stored in the token, what would happen if they were modified?

</details>

---

<details>
<summary><strong>Hint 3</strong></summary>

Normally, authentication tokens are protected by a **signature** to prevent tampering.

Check whether the application actually verifies that signature.

</details>

---

<details>
<summary><strong>Final Hint</strong></summary>

Try modifying the decoded data and replacing the token stored in your browser.

If the server accepts the modified token, you may gain additional permissions within the application.

</details>
