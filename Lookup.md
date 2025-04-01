# Test your enumeration skills on this boot-to-root machine.

**Lookup** offers a treasure trove of learning opportunities for aspiring hackers. This intriguing machine showcases various real-world vulnerabilities, ranging from web application weaknesses to privilege escalation techniques. By exploring and exploiting these vulnerabilities, hackers can sharpen their skills and gain invaluable experience in ethical hacking. Through "Lookup," hackers can master the art of reconnaissance, scanning, and enumeration to uncover hidden services and subdomains. They will learn how to exploit web application vulnerabilities, such as command injection, and understand the significance of secure coding practices. The machine also challenges hackers to automate tasks, demonstrating the power of scripting in penetration testing.

Full walkthrough: https://github.com/garthheff/CTF-Writeups/blob/main/TryHackMe/Lookup.md
# What is the user flag?

<details>
  <summary><strong>Hint on how to make site load</strong></summary>

Add lookup.thm to /etc/hosts file
</details>

<details>
  <summary><strong>1st hint on first entry point</strong></summary>

You can emulate usernames (successful usernames will give a different error)

wget https://raw.githubusercontent.com/danielmiessler/SecLists/refs/heads/master/Usernames/Names/names.txt
</details>

<details>
  <summary><strong>2st hint on first entry point</strong></summary>

admin user might not be the one you are looking for
</details>

<details>
  <summary><strong>3rd hint on first entry point</strong></summary>

Password for the user in question is in most password lists such as rockyou
</details>

<details>
  <summary><strong>1st hint on second entry point</strong></summary>
add files.lookup.thm to the hosts file if not already
search for exploit of web application on exploit-db 
</details>

<details>
  <summary><strong>2nd hint on second entry point</strong></summary>

exploit CVE-2019-9194 , while script from exploit-db works, metasploit has a working exploit   
</details>

<details>
  <summary><strong>Small 1st hint on accessing flag</strong></summary>

the www-data user does not have access to the flag. But does it have access to any files that have more permissions?

</details>

<details>
  <summary><strong>Bigger 1st hint on accessing flag, if still stuck</strong></summary>

Check for files with SUID or SGID bits set that are not a common file, linpeas script will find and suggest it's an uncommon file

</details>

<details>
  <summary><strong>Small hint on exploiting this file</strong></summary>

Run file, is it using other commands that could be exploited? what is it looking for and why? 

</details>

<details>
  <summary><strong>Big hint on exploiting this file</strong></summary>

This file appears to be a basic "password manager" that reads a user's `.password` file from their home directory. It uses the `id` command to determine which user’s file to read and prints it to the screen.

You can run `id username` to see what the expected output looks like. Then, try crafting your own version of the `id` command that produces similar output. Finally, use a well-known technique to ensure your custom `id` command is run instead of the system’s default.

</details>

<details>
  <summary><strong>Small hint on exploiting this file</strong></summary>

Run file, is it using other commands that could be exploited? what is it looking for and why? 

</details>

<details>
  <summary><strong>Final hint</strong></summary>

Once the password list is obtained, attempt to SSH in with that user and passwords from the list until you connect. good practice for hydra or similar. 

Once connected you should have access to read the flag

</details>
## What is the root flag?

<details>
  <summary><strong>Hint 1</strong></summary>

Can we run any files as sudo without requiring a password?

</details>

<details>
  <summary><strong>Hint 2</strong></summary>

Gtfobins

</details>

<details>
  <summary><strong>Hint 3 - capture flag the easy way</strong></summary>

Flag location and name is the normal for a root flag

</details>

<details>
  
<summary><strong>Hint 4 - capture flag a better way</strong></summary>

How can a user SSH without a password? 

</details>