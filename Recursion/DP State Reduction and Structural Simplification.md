## Lessons from Maximum Points You Can Obtain from Cards

### 1. Not every changing variable is a state variable

During recursion, I had three changing variables:

```text
(i, j, k)
```

Initially, I considered all three as DP dimensions.

But `k` could be derived from `i` and `j`:

```text
k = n - i - j - 1
```

Therefore `k` does not provide new information.

### Rule

> **A variable should be a DP state variable only when it contains information needed to determine the future and cannot be derived from the other state variables.**

Before creating a new DP dimension, ask:

```text
Can I derive this variable from the existing state?
```

If yes → don't add another dimension.

---

## 2. Look at the structure of the choices before using DP

In this problem, cards can only be picked from:

```text
left end OR right end
```

Therefore, after picking `k` cards, the selected cards always form:

```text
prefix + suffix
```

Equivalently, the cards that remain unselected form:

```text
one contiguous middle subarray
```

This gives a transformation:

```text
maximize selected sum
=
total sum - minimize remaining contiguous window sum
```

where the remaining window has fixed size:

```text
n - k
```

This turns the problem into a sliding-window problem.

### Trigger

When I see:

- continuous selection from one/both ends
    
- prefix + suffix
    
- fixed number of elements selected
    
- contiguous portion left behind
    

I should immediately ask:

> **Can I represent the problem using one contiguous window instead of tracking every choice?**

---

## General Problem-Solving Habit

Don't stop after finding a valid DP formulation.

After getting a DP solution, ask:

1. **Can any state variable be derived?**
    
2. **Can I reduce the number of dimensions?**
    
3. **Does the choice structure imply a simpler representation?**
    
4. **Can the problem become greedy, two pointers, sliding window, prefix sum, etc.?**
    
5. **Do the constraints make my current approach unnecessarily expensive?**
    

### Core lesson

> **First minimize the state; then question whether you need the state at all.**