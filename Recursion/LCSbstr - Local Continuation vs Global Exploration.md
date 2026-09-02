## The Mistake

In **Longest Common Substring**, I defined:

```text
helper(i, j) = length of the common substring starting exactly at (i, j)
```

When:

```text
s1[i] == s2[j]
```

I initially only explored:

```text
helper(i + 1, j + 1)
```

because I was thinking:

> "The characters match, so continue the current substring."

But this missed other possible starting positions.

## Why This Is Wrong

When `s1[i] == s2[j]`, there are actually two different things to consider:

### 1. Continue the current substring

```text
(i, j) → (i+1, j+1)
```

This tells us how long the substring starting at `(i,j)` can become.

### 2. Search for another substring

```text
(i+1, j)
(i, j+1)
```

These can lead to a completely different starting position that may produce a longer answer.

Therefore, a match does **not** mean we can stop exploring the other states.

## Example

```text
s1 = "abbedabdaec"
s2 = "bebccc"
```

At:

```text
(i,j) = (1,0)
```

we have:

```text
s1[1] = 'b'
s2[0] = 'b'
```

So we can continue diagonally:

```text
(1,0) → (2,1)
```

But this gives only length `1`.

The actual answer `"be"` starts at:

```text
(2,0)
```

If we stop exploring when `(1,0)` matches, we never discover it.

## Correct Mental Model

For a matching state:

```text
s1[i] == s2[j]
```

there are **two responsibilities**:

```text
currLen
    ↓
How long can the substring starting HERE become?
    → 1 + helper(i+1, j+1)

global exploration
    ↓
Could another starting position produce a better answer?
    → helper(i+1, j)
    → helper(i, j+1)
```

For a mismatch:

```text
s1[i] != s2[j]
```

the substring starting exactly here has length:

```text
0
```

but we must still explore:

```text
(i+1,j)
(i,j+1)
```

because a common substring may begin later.

## General Lesson

> **A recursive call may have a local successful path while other branches can still contain a better global answer. Do not confuse "I found a valid continuation" with "I have found the global optimum."**

This is especially important when a recursive function is doing both:

1. calculating the answer for the current state, and
    
2. exploring the state space to find the global answer.
    

## Pattern to Watch For

Whenever I write:

```text
if (condition)
    return recursiveCall(...)
else
    explore other choices
```

I should stop and ask:

> **Can the other choices still contain a better answer even though this condition succeeded?**

If yes, I must explore those branches too.