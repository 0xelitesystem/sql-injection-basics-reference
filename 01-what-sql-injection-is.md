# What SQL injection is

SQL injection is a vulnerability where untrusted input is combined into a SQL query in a way that lets the input change what the query does. Instead of being treated as a plain value, the attacker's text becomes part of the command the database runs. This happens whenever a program builds query text by concatenating strings that include data from a user, a URL, a form, or any other source outside the program's control.

## Why it works

A database receives a single string of SQL and parses it into structure (the commands, tables, and conditions) and values (the data). When you build that string by pasting user input into the middle of it, the database cannot tell which parts you meant as structure and which you meant as data. It just parses the whole string. So if the input contains SQL syntax, the parser reads that syntax as part of the command. The program intended a value; the attacker supplied code, and the database obeyed the code.

## What an attacker can do

Once input can alter query structure, the consequences range widely. An attacker may bypass a login by making a WHERE clause always true, read data from tables they should never see, dump entire user tables including password hashes, modify or delete records, or in some setups run administrative commands. Injection is consistently ranked among the most serious and common web vulnerabilities precisely because a single unguarded query can expose or destroy an entire database.

## Where it hides

The classic case is a login or search box, but the vulnerable pattern appears anywhere input reaches a query: URL parameters, form fields, HTTP headers, cookies, JSON bodies, and even data read back from your own database and reused in a second query. The root cause is always the same, mixing untrusted data into query text, so the defense is always the same: keep data and query structure strictly separate, which is what the next files show.
