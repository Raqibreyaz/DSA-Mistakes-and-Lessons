## Problem

In Job Sequencing, each job has:

- a deadline
    
- a profit
    
- each job takes exactly 1 unit of time
    
- only one job can be executed in a slot
    

The greedy strategy is:

1. Sort jobs by decreasing profit.
    
2. For each job, put it in the **latest available slot <= its deadline**.
    

---

## Initial Approach

For every job, I searched backward:

```text
deadline → deadline - 1 → deadline - 2 → ...
```

This can take `O(D)` for one job.

For `N` jobs:

```text
O(N × D)
```

which can cause TLE when deadlines are large.

---

# DSU Optimization

The expensive operation is:

> Find the latest available slot ≤ `deadline`.

We can use DSU to skip slots that are already occupied.

## DSU Meaning

Define:

```text
find(x) = latest available slot <= x
```

Initially every slot is available:

```text
parent[0] = 0
parent[1] = 1
parent[2] = 2
parent[3] = 3
parent[4] = 4
...
```

Therefore:

```text
find(5) = 5
```

means slot 5 is available.

---

## What happens when a slot is occupied?

Suppose:

```text
find(5) = 3
```

We schedule the job in slot `3`.

Slot 3 is now unavailable.

The next possible slot before 3 is:

```text
find(2)
```

Therefore:

```java
parent[3] = find(2);
```

This means:

```text
3 → next available slot before 3
```

### Important

If:

```text
find(5) = 3
```

we **do not** update:

```java
parent[5]
```

because we didn't occupy slot 5.

We occupied slot **3**.

This was my mistake.

---

# Visual Example

Initially:

```text
1   2   3   4   5
|   |   |   |   |
1   2   3   4   5
```

Suppose slot 5 is occupied:

```text
5 → 4
```

Then slot 4 is occupied:

```text
5 → 4 → 3
```

Then slot 3 is occupied:

```text
5 → 4 → 3 → 2
```

So:

```text
find(5) = 2
```

The DSU allows us to jump over occupied slots instead of checking them one by one.

---

# Path Compression

The `find()` function:

```java
private static int find(int[] parent, int node) {
    if (parent[node] == node)
        return node;

    return parent[node] = find(parent, parent[node]);
}
```

also performs path compression.

If:

```text
5 → 4 → 3 → 2
```

and we call:

```text
find(5)
```

it returns `2` and compresses the path:

```text
5 → 2
4 → 2
3 → 2
```

Future searches become much faster.

---

# Correct Core Logic

```java
int slot = find(parent, job.deadline);

if (slot == 0)
    continue;

maxProfit += job.profit;
count++;

parent[slot] = find(parent, slot - 1);
```

The important line is:

```java
parent[slot] = find(parent, slot - 1);
```

because `slot` is the position we actually occupied.

---

# Complexity

Sorting:

```text
O(N log N)
```

Each DSU operation is approximately:

```text
O(α(D))
```

where `α` is the inverse Ackermann function and is effectively constant for practical input sizes.

Therefore:

```text
Total Time ≈ O(N log N)
Space = O(D)
```

---

# Main Interview Lesson

The difficult part was not knowing DSU.

The difficult part was identifying **what the DSU relationship represents**.

Before coding an unfamiliar DSU application, ask:

> **What does `parent[x]` mean?**

For this problem:

> `find(x)` gives the latest available slot at or before `x`.

Then ask:

> **What changes when I consume `x`?**

Answer:

> `x` becomes unavailable, so connect it to the next available slot before it.

Therefore:

```text
parent[x] = find(x - 1)
```

---

# General Pattern

This DSU technique is useful when:

- positions/slots are consumed
    
- once consumed, they should be skipped
    
- we repeatedly need the next available position
    
- we want to avoid repeatedly scanning occupied positions
    

Think:

```text
"Find the next available position"
        ↓
DSU can skip already-used positions
```

Examples include:

- Job Sequencing
    
- Assigning resources to the latest available slot
    
- Filling positions while skipping occupied ones
    
- Offline "delete and find predecessor" problems
    

---

# Mistake to Avoid

Don't confuse:

```text
job.deadline
```

with:

```text
actual slot occupied
```

The deadline is only the **upper bound**.

Example:

```text
deadline = 5
find(5) = 3
```

means:

```text
Job can use up to slot 5
but slot 3 is the latest currently available slot
```

Therefore, after scheduling:

```text
slot 3 becomes unavailable
```

not slot 5.