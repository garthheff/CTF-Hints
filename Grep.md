# Grep - A challenge that tests your reconnaissance and OSINT skills.
https://tryhackme.com/room/greprtp

Welcome to the OSINT challenge, part of TryHackMe’s Red Teaming Path. In this task, you will be an ethical hacker aiming to exploit a newly developed web application.

SuperSecure Corp, a fast-paced startup, is currently creating a blogging platform inviting security professionals to assess its security. The challenge involves using OSINT techniques to gather information from publicly accessible sources and exploit potential vulnerabilities in the web application.

Start by deploying the machine; Click on the `Start Machine` button in the upper-right-hand corner of this task to deploy the virtual machine for this room.

Your goal is to identify and exploit vulnerabilities in the application using a combination of recon and OSINT skills. As you progress, you’ll look for weak points in the app, find sensitive data, and attempt to gain unauthorized access. You will leverage the skills and knowledge acquired through the Red Team Pathway to devise and execute your attack strategies.

## What is the API key that allows a user to register on the website?

<details>
  <summary><strong>Hint #1</strong></summary>
nmap and find website site, hostname of site can be found on website certificate
</details>

<details>
  <summary><strong>Hint #2</strong></summary>
Enumerate website, get a feel for it's structure 
</details>

<details>
  <summary><strong>Hint #3</strong></summary>
OSINT using data extracted from room description and information found from main website
</details>

<details>
  <summary><strong>Bigger hint if #3 was not enough</strong></summary>
Find private github repository for web application by the company in room description 
</details>

<details>
  <summary><strong>Hint #4</strong></summary>
Search through commits
</details>

## What is the first flag?

<details>
  <summary><strong>Hint #1</strong></summary>
Register a user with the API key found in previous flag
</details>

<details>
  <summary><strong>Bigger hint if #1 was not enough</strong></summary>
Capture a bad user registration post with browser developer tools or burp suite etc, update to use api key we found on first flag and resend. 
</details>

<details>
  <summary><strong>Hint #2</strong></summary>

Login
</details>

## What is the email of the "admin" user?

<details>
  <summary><strong>Hint #1</strong></summary>
Upload a reverse shell and execute it, review the upload.php within the github for ideas on how 
</details>

<details>
  <summary><strong>Bigger hint if #1 was not enough</strong></summary>
PHP shell, update magic byte to that of one of the accepted images
</details>

<details>
  <summary><strong>Hint #2</strong></summary>
email address is within a file in a common directory
</details>

<details>
  <summary><strong>Bigger hint if #2 was not enough</strong></summary>
search for backup folders
</details>

## What is the host name of the web application that allows a user to check an email for a possible password leak?

<details>
  <summary><strong>Hint #1</strong></summary>
More than website, find it and check it's certificate
</details>

<details>
  <summary><strong>Bigger hint if #1 was not enough</strong></summary>
nmap every port e.g -p- switch find a webserver on a non standard port outside the range of a default port range of nmap
</details>

## What is the password of the "admin" user?

<details>
  <summary><strong>Hint #1</strong></summary>
You found the admin email, you found the leak site, put them both together :)
</details>