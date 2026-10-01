## Counting Objects Through a Maximum-Size DP

### Core Insight

A DP state does **not necessarily need to return the quantity asked by the problem**.

For the **Count Square Submatrices With All Ones** problem:

```text
f(i, j) = maximum size of an all-1 square starting at (i, j)
```

If:

```text
f(i, j) = k
```

then the following squares are all valid:

```text
1 × 1
2 × 2
...
k × k
```

Therefore, exactly `k` valid squares start at `(i,j)`.

So:

```text
total count = Σ f(i,j)
```

### Why This Works

A valid square of size `k` automatically contains valid squares of every smaller size starting from the same top-left cell.

Therefore:

```text
maximum size = number of valid square sizes
```

### Recurrence

If:

```text
matrix[i][j] == 0
```

then:

```text
f(i,j) = 0
```

Otherwise:

```text
f(i,j) =
1 + min(
    f(i+1,j),
    f(i,j+1),
    f(i+1,j+1)
)
```

The three neighboring states determine how large the square can grow downward, rightward, and diagonally.

### General Pattern

When a problem asks to **count valid substructures**, don't assume the DP state must directly represent a count.

Ask:

> "Can one DP value implicitly represent multiple valid objects?"

For this problem:

```text
f(i,j) = 4
```

implicitly represents:

```text
1×1, 2×2, 3×3, 4×4
```

and therefore contributes `4` to the answer.

### Problem-Solving Lesson

My initial brute-force approach correctly identified repeated work, but I focused on enumerating every square explicitly.

A better question is:

> "What reusable information about each starting position lets me know how many valid squares originate there?"

That leads naturally to the maximum-size DP.
