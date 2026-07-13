# Vulnerable versus safe example

The difference between a vulnerable query and a safe one is whether the input is concatenated into the query text or passed as a separate parameter. Below is a login lookup written both ways. The vulnerable version builds one string; the safe version sends a query template plus values.

## The vulnerable version

```python
# UNSAFE: user input is concatenated into the SQL string
username = request.form["username"]
password = request.form["password"]

query = "SELECT * FROM users WHERE username = '" + username + \
        "' AND password = '" + password + "'"
cursor.execute(query)
```

If an attacker types this as the username:

```
' OR '1'='1
```

the query the database actually runs becomes:

```sql
SELECT * FROM users WHERE username = '' OR '1'='1' AND password = ''
```

The condition `'1'='1'` is always true, so the WHERE clause matches rows regardless of the real credentials. The login check is defeated because the input became part of the logic.

## The safe version

```python
# SAFE: query template and values are sent separately
username = request.form["username"]
password = request.form["password"]

query = "SELECT * FROM users WHERE username = %s AND password = %s"
cursor.execute(query, (username, password))
```

Here the `%s` placeholders mark where values go. The driver sends the query structure and the values to the database as distinct things. The same `' OR '1'='1` input is now treated as a literal username string to compare against, not as SQL. It simply will not match any real username, so the login fails safely.

## The takeaway

The vulnerable and safe versions do the same job, but only the safe one keeps the attacker's text on the data side of the line. That separation is the entire defense, and it is described in detail next.
