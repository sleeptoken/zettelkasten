
2024-05-26 13:52

Source: #tryhackme 

Tags: [[database]]


Flat-file databases are stored as a file on the disk of a computer.
The most common (and simplest) format of a flat-file database is an SQLite database.

Has a dedicated client for querying them on the command line. This client is called `sqlite3`

To access it, we use `sqlite3 <database-name>`
we can see the tables in the database by using the `.tables` command
use `PRAGMA table_info(table_name);` to see the table information.
Then we'll use `SELECT * FROM table_name;` to dump the information from the table



### References
