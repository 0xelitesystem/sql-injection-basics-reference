# SQL Injection Basics

A plain-language reference on SQL injection: the vulnerability that lets an attacker change the meaning of a database query by feeding it crafted input. It covers what the attack is, how string-concatenated queries get exploited, why parameterized queries are the real fix, why escaping alone is not enough, and how least privilege limits the damage. The framing is defensive and educational.

## The core idea

SQL injection happens when untrusted input is glued directly into a SQL query string, so the database treats attacker data as query code instead of as a value. The fix is to send the query and the data separately using parameterized queries (prepared statements), so the input can never change the query structure no matter what characters it contains. Escaping and input filtering help around the edges but are not a substitute for parameterization, and running the database account with least privilege limits how much a successful attack can reach.

## What is inside

- [01-what-sql-injection-is.md](01-what-sql-injection-is.md) the vulnerability in plain terms.
- [02-vulnerable-vs-safe-example.md](02-vulnerable-vs-safe-example.md) concrete concatenated query versus a parameterized one.
- [03-parameterized-queries.md](03-parameterized-queries.md) how prepared statements actually stop it.
- [04-why-escaping-is-not-enough.md](04-why-escaping-is-not-enough.md) the limits of escaping and blocklists.
- [05-least-privilege-and-defense-in-depth.md](05-least-privilege-and-defense-in-depth.md) limiting the blast radius.

## The stance

This reference is defensive and educational. It shows a vulnerable pattern only so you can recognize and remove it, and it treats parameterized queries as the default correct way to talk to a database. Escaping, validation, and least privilege are supporting layers, not replacements for parameterization.

## More

Part of a catalog of single-file browser tools and plain-language references, all MIT licensed and dependency-free: [0xelitesystem.github.io](https://0xelitesystem.github.io/). Built by [elitesystem.ai](https://elitesystem.ai).

## License

MIT. Copyright RBL Industries LLC 2026.
