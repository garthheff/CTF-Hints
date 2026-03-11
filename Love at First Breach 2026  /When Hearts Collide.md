# When Hearts Collide

Room: [https://tryhackme.com/room/lafb2026e1](https://tryhackme.com/room/lafb2026e1)

---

# Scenario

**Matchmaker** is a playful web application that pairs you with a dog by comparing **MD5 hashes** of uploaded images.

Upload a photo and the application generates an MD5 fingerprint.
If the fingerprint matches one of the hashes in their curated dog database, you get a **match**.

The challenge description hints that the algorithm is transparent and deterministic.

---

<details> <summary><strong>Hint 1</strong></summary>

Look carefully at the challenge title.

Sometimes the name of the room itself is trying to point you toward the vulnerability.

</details>
<details> <summary><strong>Bigger Hint 1</strong></summary>

The word “Collide” is important.

Think about where the concept of a collision appears in cybersecurity.

</details>

<details> <summary><strong>Hint 2</strong></summary>

The site uses a hashing algorithm to determine if two uploads match.

Research weaknesses in MD5 hashing.

</details>
<details> <summary><strong>Hint 3</strong></summary>

You do not need to generate a collision yourself.

There are publicly available example files that already share the same MD5 hash.

</details>
