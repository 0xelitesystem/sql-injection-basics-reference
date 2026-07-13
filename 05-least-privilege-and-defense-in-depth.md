# Least privilege and defense in depth

Least privilege means the database account your application uses should have only the permissions it actually needs, so that even a successful injection can reach as little as possible. It does not prevent injection, but it caps the damage, which is why it belongs in every deployment alongside parameterized queries.

## Why it matters

If your application connects to the database as an all-powerful admin user, a single injection can read every table, alter schema, drop tables, or in some engines run system commands. If instead the account can only SELECT, INSERT, and UPDATE the specific tables the app uses, the same injection is boxed into a far smaller space. The vulnerability is the same; the blast radius is not. Least privilege turns a catastrophe into a contained incident.

## Practical steps

- Give the application account rights only to the specific databases and tables it uses, and only the operations it needs. Many apps never need DROP or ALTER at runtime.
- Do not use the database superuser or root account for normal application traffic.
- Separate accounts by role where it helps: a read-only reporting connection cannot be used to modify data even if it is exploited.
- Keep schema-changing migrations under a separate, tightly controlled account that the running app does not use.

## Defense in depth

Least privilege is one layer among several. Combine it with parameterized queries as the core defense, input validation as an early filter, and careful error handling so the database does not leak table names, query text, or stack traces to attackers in error messages. Log and monitor for unusual query patterns. No single layer is perfect, so you stack them: the query separation stops the attack, validation trims bad input early, least privilege limits what any breach can touch, and quiet error handling denies attackers the feedback they use to refine an attack. Together they make a database hard to exploit and cheap to defend.
