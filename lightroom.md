# **Easy - Welcome to the Light database application!**
https://tryhackme.com/room/lightroom

I am working on a database application called Light! Would you like to try it out?
If so, the application is running on port 1337. You can connect to it using ``nc 10.10.22.91 1337``
You can use the username smokey in order to get started.

<details>
  <summary><strong>Hint on what to exploit</strong></summary>
  
  - SQLi on the username input
</details>

<details>
  <summary><strong>Hint for the SQLi 1</strong></summary>
  
  - Custom filters to overcome, e.g can't use --
</details>

<details>
  <summary><strong>Hint for the SQLi 2</strong></summary>
  
  - can't use -- although can correct syntax for most with using ' as ending
</details>

<details>
  <summary><strong>Hint for the SQLi 3</strong></summary>
  
  - type upper and lower case in combo for filtered commands
</details>

<details>
  <summary><strong>Hint for the SQLi 4</strong></summary>
  
  - Rearch SQL operations that combines the results of two or more SELECT statements into a single result set.
</details>

<details>
  <summary><strong>Hint - What SQL version is it?</strong></summary>
  
  - MySQL: ``SELECT @@version``
  - Postgre: ``SQL SELECT version()``
  - SQLite: ``SELECT sqlite_version()``
  - MSSQL: ``SELECT @@version``
</details>

<details>
  <summary><strong>Hint - Extract data</strong></summary>
  
  - Research how to extract table names
  - Research how to extract the SQL to create that table
  - Research how select data from the columns found within the creation SQL

</details>
