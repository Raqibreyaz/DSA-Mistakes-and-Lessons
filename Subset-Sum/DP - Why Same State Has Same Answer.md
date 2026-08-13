## Core Idea

> **Same state → same future → same answer**

Memoization is valid only when the future depends on the current state, not on the history used to reach that state.

---

## What is a DP State?

A DP state contains all the information needed to make future decisions.

For example:

```text
helper(index, sum)
```

The state is:

```text
(index, sum)
```

- `index` tells us which elements are still available.
    
- `sum` tells us the current accumulated sum.
    

---

## Why Does the Same State Give the Same Answer?

Suppose two different paths reach:

```text
(index = 4, sum = 7)
```

The paths may have selected different elements to reach sum `7`.

For example:

```text
Path A → selected elements sum to 7
Path B → selected elements sum to 7
```

But once both reach:

```text
index = 4
sum = 7
```

they have exactly:

- the same remaining elements: `arr[4 ... n-1]`
    
- the same current sum: `7`
    
- the same possible future choices
    

Therefore, both paths will make the same recursive decisions from this point onward.

So:

```text
helper(4, 7)
```

must return the same answer for both paths.

The history used to reach `(4, 7)` no longer matters.

---

## Visual Intuition

```text
Different histories
       /       \
      /         \
 Path A         Path B
      \         /
       \       /
       (index, sum)
            |
            |
       Same future
            |
       Same answer
```

---

## Mathematical View

Let:

```text
F(i, s)
```

represent the answer from index `i` with current sum `s`.

The next decisions are:

```text
Take:
F(i + 1, s + arr[i])

Don't take:
F(i + 1, s)
```

If two paths reach the same `(i, s)`, both have exactly the same two next states.

Therefore they produce the same result.

---

## How to Test a DP State

Ask:

> **If I erase the entire history and keep only my proposed state variables, can I still make exactly the same future decisions?**

### If YES

The state contains enough information.

Memoization is valid.

### If NO

Something is missing from the state.

Add another dimension/variable.

---

## Example of an Invalid State

Suppose we only use:

```text
dp[index]
```

Two paths can reach:

```text
(index = 4, sum = 5)
(index = 4, sum = 10)
```

The future answer can be different because the current sum affects the final partition difference.

Therefore:

```text
dp[index]
```

is not enough.

We need:

```text
dp[index][sum]
```

---

## The Most Important Rule

> **A DP state is valid when the future depends only on the state, not on the history used to reach it.**

Short version:

> **Same state → same future → same answer.**

This is the fundamental reason memoization works.

---

## When Solving a New DP Problem

Don't immediately ask:

> "What DP pattern is this?"

Instead ask:

1. What are my decisions?
    
2. What is my brute-force recursion?
    
3. What information describes my current situation?
    
4. Can two different paths reach the same state?
    
5. If they do, will their future be identical?
    
6. If yes → overlapping subproblems → memoization can help.
    
7. How many possible states are there?