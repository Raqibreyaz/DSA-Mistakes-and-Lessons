## Mistake I Used To Make

I started recursion problems by thinking:

- What should helper() return?
- What parameters should helper() take?

This caused confusion in problems like Letter Combinations because I tried to design the function before understanding the search process.

---

## Better Approach

Never start from the helper signature.

Instead, answer these questions in order.

### 1. What is the problem asking me to build?

Examples

- Generate Parentheses -> A valid parentheses string
- Letter Combinations -> A letter combination
- Combination Sum -> A valid combination of numbers
- N Queens -> A valid board

---

### 2. What is one recursive call responsible for?

One recursive call is **not** responsible for making one decision.

It is responsible for

> Generating every valid solution that extends the current partial solution.

Examples

Letter Combinations

> Generate every valid letter combination starting from the current partial combination.

Combination Sum

> Generate every valid combination extending the current partial combination.

---

### 3. Discover the State

Imagine pausing the algorithm.

Ask:

> What information must I preserve so I can continue from this exact point?

That information becomes the recursive state.

Examples

Generate Parentheses

State

- current string
- open used
- close used

Letter Combinations

State

- current combination
- current digit index

Combination Sum

State

- current combination
- current sum
- start index

---

### 4. Choices

What decisions are available from the current state?

Examples

Generate Parentheses

- add '('
- add ')'

Letter Combinations

- choose any mapped character

Combination Sum

- choose any candidate from startIndex onward

---

### 5. Rules

What makes a choice valid?

Generate Parentheses

- open < n
- close < open

Combination Sum

- sum <= target
- don't go before startIndex

---

### 6. Base Case

When has this branch completely finished exploring?

Not

"When recursion ends."

Instead

"When this branch has become a complete valid solution."

## Important Insight

The recursive state is NOT invented.

The recursive state is discovered.

Ask:

"If I paused the algorithm right now, what information would I absolutely need to continue?"

Those pieces of information become the recursive state.