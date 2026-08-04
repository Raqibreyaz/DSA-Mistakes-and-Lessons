## BFS - Mark Visited on Enqueue

### Problem

While solving **Rotten Oranges**, I got a TLE even though I was using Multi-Source BFS.

---

### Root Cause

I only checked whether a cell was rotten before adding it to the queue.

I never marked a fresh orange as rotten immediately after enqueueing it.

This allowed multiple neighbouring rotten oranges to enqueue the same fresh orange multiple times.

Example:

```text
2 1
1 1
```

Without marking visited immediately,

- Top orange enqueues bottom-right.
    
- Left orange also enqueues bottom-right.
    

The same node enters the queue multiple times.

---

### Correct BFS Invariant

> **The moment a node is pushed into the queue, it must be marked as visited.**

Never wait until it is removed from the queue.

This guarantees every node enters the queue at most once.

---

### Why?

BFS explores nodes level by level.

Before a node gets popped, another path may discover it.

If it isn't marked visited yet, it gets inserted again.

That causes

- Duplicate work
    
- Larger queue
    
- TLE
    

---

### Rule to Remember

Whenever writing BFS, ask:

> **Can another node enqueue this node before it gets processed?**

If yes,

**Mark it visited immediately when enqueueing.**

Never when dequeuing.

---

### Edge Case Found

Initially I returned `-1` when the queue was empty.

This fails for

```text
0 0
0 0
```

Expected answer:

```text
0
```

because there are no fresh oranges to rot.

A cleaner solution is:

- Count fresh oranges during the initial scan.
    
- If `fresh == 0`, return `0`.
    
- Decrement `fresh` whenever an orange becomes rotten.
    
- At the end:
    
    - `fresh == 0` → return minutes.
        
    - Otherwise → `-1`.
        

---

### Pattern

- Multi-Source BFS
    
- Grid BFS
    
- Visited State Management
    

---

### General Takeaway

> **In BFS, "visited" means "already discovered", not "already processed".**

Mark nodes visited **when discovered (enqueue)**, not when processed (dequeue).