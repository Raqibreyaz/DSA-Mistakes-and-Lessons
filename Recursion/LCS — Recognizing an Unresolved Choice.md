## The Mistake

When `s1[i] != s2[j]`, my first thought was:

> Search for `s1[i]` in the remaining part of `s2`, or search for `s2[j]` in the remaining part of `s1`.

I was trying to decide immediately which character should be used.

The problem is that **I cannot know locally which character should be skipped**.

A character appearing later does not mean that choosing it is part of the optimal LCS.

## The Correct Intuition

At `(i, j)`:

```text
s1[i] != s2[j]
```

There are two possible decisions:

```text
1. Skip s1[i]
   → (i + 1, j)

2. Skip s2[j]
   → (i, j + 1)
```

Both are possible because either character could be the one preventing the optimal subsequence.

Therefore, I must explore both paths and keep the better result.

## Why Recursion Is Needed

After choosing one path, I still face the same type of decision.

For example:

```text
(i, j)
   |
   +-- skip s1[i] → (i+1, j)
   |                    |
   |                    +-- maybe skip s1[i+1]
   |                    +-- maybe skip s2[j]
   |
   +-- skip s2[j] → (i, j+1)
                        |
                        +-- maybe skip s1[i]
                        +-- maybe skip s2[j+1]
```

So I cannot simply move both pointers forward.

Each new state can contain another unresolved choice.

## Equal Characters

If:

```text
s1[i] == s2[j]
```

then this character can contribute to the common subsequence:

```text
1 + LCS(i + 1, j + 1)
```

There is no need to branch at this point.

## The General DSA Lesson

When solving an optimization problem recursively:

> **If I cannot determine which choice is optimal from the current information, don't guess. Preserve the possible choices and compare their results.**

Mental pattern:

```text
Unresolved choice
      ↓
Multiple possible decisions
      ↓
Explore each decision
      ↓
Solve remaining problem
      ↓
Take best result
```

This is the important intuition behind many recursion + DP problems.

## Trigger for Future Problems

When I catch myself saying:

> "I'll just choose this option because it looks better."

I should stop and ask:

> **"Do I actually have enough information to prove this choice is optimal?"**

If not, I should look for the competing choices and determine whether both need to be explored.