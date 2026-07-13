# Why escaping is not enough

Escaping and blocklisting are weaker defenses than parameterization because they try to sanitize dangerous input rather than remove the mixing of data and code entirely. They can reduce risk, but they are error prone and have well known gaps, so they should never be your primary defense against SQL injection.

## Escaping tries to patch the symptom

Escaping means adding backslashes or doubling quotes so that a quote in the input does not close the string in your query. The problem is that you are still building one combined string and hoping you neutralized every character that matters. Get one context wrong and the protection fails. Parameterization removes the need to escape at all, because the value never lives inside the query text in the first place. Escaping fights the symptom; parameterization removes the cause.

## Concrete ways escaping fails

- Numeric contexts have no quotes to escape. If you write `WHERE id = " + userId` and the value is not quoted, escaping quotes does nothing, and input like `1 OR 1=1` walks straight in.
- Character set and encoding mismatches have historically let crafted multibyte sequences slip a quote past a naive escaper.
- Escaping the wrong function for the context, or forgetting to escape one of several inputs, reintroduces the hole. Humans miss cases; a single missed one is exploitable.

## Blocklists are worse

Blocking words like `SELECT`, `DROP`, or `--` is even more fragile. Attackers bypass keyword filters with mixed case, comments inserted between characters, alternate encodings, and equivalent syntax. Blocklists also break legitimate input (a search for the word "select" or a name like O'Brien) while still missing real attacks. You cannot enumerate every dangerous pattern.

## The right layering

Use parameterized queries as the actual defense. Use input validation (allowlists of expected format, length, and type) as a helpful early filter that catches obviously bad input and enforces business rules. Validation and escaping are supporting layers on top of parameterization, never a replacement for it.
