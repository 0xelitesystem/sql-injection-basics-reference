# Parameterized queries

A parameterized query (also called a prepared statement) is a query where the SQL text contains placeholders instead of values, and the values are supplied separately when the query runs. Because the database receives the query structure first and the data second, the data can never be parsed as part of the command. This is the primary, reliable fix for SQL injection.

## How it works underneath

When you prepare a statement, the database parses and plans the query using the placeholders as markers for where values will go. The structure is fixed at that moment. When you then bind values, they are delivered through a separate channel and slotted into those marked positions as pure data. The parser has already finished; it never re-reads the values as SQL. So a value like `' OR '1'='1` is stored and compared as the literal characters they are, with no special meaning. There is no string for an attacker to break out of.

## What placeholders look like

Different drivers use different placeholder syntax, but the idea is identical:

```sql
-- named placeholders
SELECT * FROM orders WHERE customer_id = :cid AND status = :status

-- positional placeholders (question mark style)
SELECT * FROM orders WHERE customer_id = ? AND status = ?
```

You then bind values by name or position. You never build the query by concatenation, and you never quote the values yourself, the driver handles that correctly.

## Rules that keep it safe

Parameterize every value that comes from outside the program, every time, with no exceptions for values that "look safe." One concatenated query is enough to be vulnerable. Note one real limit: placeholders bind values, not identifiers. You cannot parameterize a table or column name or a keyword like ASC. When those must be dynamic, validate them against a fixed allowlist of known-good names rather than concatenating input. For every actual value, though, the parameter is the answer, and it works regardless of what characters the input contains.
