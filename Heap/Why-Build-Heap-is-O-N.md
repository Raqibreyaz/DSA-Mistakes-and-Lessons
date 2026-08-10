## Common Misconception

When building a heap using **bottom-up heapify**, it's tempting to think:

```text
n/2 non-leaf nodes
×
log n work per heapify

= O(n log n)
```

This is **incorrect**.

The mistake is assuming every `heapifyDown()` travels all the way to the bottom.

---

## Key Observation

Different nodes have different maximum travel distances.

```
                 Root
              /        \
          Level 1     Level 1
          /    \      /     \
      Level2  Level2 ...
```

- Root may move all the way down.
    
- Nodes near the root move several levels.
    
- Nodes near the leaves move only one level.
    
- Leaves never move.
    

Most nodes are already very close to their final position.

---

## Work Per Height

|Height from Bottom|Approx. Nodes|Max Heapify Cost|Total Work|
|---|--:|--:|--:|
|0 (Leaves)|n/2|0|0|
|1|n/4|1|n/4|
|2|n/8|2|2n/8|
|3|n/16|3|3n/16|
|...|...|...|...|

Total work:

```
n/4 + 2n/8 + 3n/16 + ...
```

Factor out `n`:

```
n × (1/4 + 2/8 + 3/16 + ...)
```

The remaining infinite series converges to a constant.

Therefore:

```
Time Complexity = O(n)
```

---

## Intuition

The expensive heapify operations happen only for a few nodes near the root.

Most nodes are near the leaves and require almost no work.

```
Few expensive operations
+
Many cheap operations
=
Overall O(n)
```

---

## When This Applies

This analysis is valid whenever we build a heap **bottom-up**.

Examples:

- Build Heap
    
- Convert Min Heap → Max Heap
    
- Heap Sort (Heap Construction Phase)
    
- `PriorityQueue(Collection)` / Heap constructors
    

---

## Contrast with Repeated Insertion

Building a heap by repeatedly inserting elements:

```java
for (int x : arr)
    insert(x);
```

Each insertion costs `O(log n)`.

```
n insertions
×
log n

= O(n log n)
```

---
# Interview Rule

> **Bottom-up heap construction is O(n).**
> 
> **Repeated insertions are O(n log n).**

---
> **Whenever you see repeated operations on every node, ask:**
> 
> _"Does every node actually incur the worst-case cost?"_
> 
> If the answer is **no** (as in Build Heap, DFS over trees, amortized stack operations, etc.), don't multiply `number of nodes × worst-case cost` blindly. Instead, analyze how much work is done **across all nodes**.