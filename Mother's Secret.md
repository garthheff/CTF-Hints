
Exploit flaws found in Mother's code to reveal its secrets.
https://tryhackme.com/room/codeanalysis


<details><summary><strong>API endpoint hint 1</strong></summary>

  - POST /yaml
  - Body field file_path
  - Reads ./public/file_path and returns parsed YAML
  - Emits a websocket event on namespace slash yaml </details>

<details><summary><strong>API endpoint hint 2</strong></summary>
  
  - POST /api/nostromo  
  - Body field file_path  
  - Reads ./public/file_path and returns file contents  
  - Sets Nostromo auth flag true  
  - Emits a websocket event on namespace slash nostromo
   </details>
   
<details><summary><strong>API endpoint hint 3</strong></summary>
    
  - POST /api/nostromo/mother  
  - Body field file_path  
  - Reads ./mother/file_path and returns file contents  
  - Requires you to have hit both the nostromo and yaml routes first due to auth flags
  </details>

<details><summary><strong>What is the number of the emergency command override?</strong></summary>

  - Read the Operating Manual</details>

<details><summary><strong>What is the special order number?</strong></summary>

  - Emergency command override</details>

<details><summary><strong>What is the special order number?</strong></summary>

- /yaml response once working will guide you </details>

<details><summary><strong>What is the hidden flag in the Nostromo route?</strong></summary>

- /yaml response once working will guide you </details>

<details><summary><strong>What is the name of the Science Officer with permissions?</strong></summary>

- Check back to the main site</details>

<details><summary><strong>What are the contents of the classified "Flag" box?</strong></summary>

- Check back to the main site</details>

<details><summary><strong>Where is Mother's secret?</strong></summary>

- Post to the last API endpoint with a file, hint of what the file is, is within the task 2 information on the challenge and hint in question name </details>

<details><summary><strong>What is Mother's secret?</strong></summary>

- Path Traversal</details>
