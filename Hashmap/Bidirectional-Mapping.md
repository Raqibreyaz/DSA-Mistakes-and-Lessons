**Mistake I made**

> I assumed a character `'x'` in `s` and `'x'` in `t` are the same entity.

**Lesson**

- Treat characters from different strings as different namespaces.
- If the problem requires a bijection, maintain:
    - `source → target`
    - `target → source`
- Never reuse one data structure for both directions unless the domains are actually the same.

**Questions where this appears**

- Isomorphic Strings
- Word Pattern
- Alien Dictionary (parts of it)
- Any bijection problem