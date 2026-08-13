# Constraint Check Before Implementation

## Why this matters

A correct algorithm can still fail because the implementation does not respect the input constraints.

Before coding, spend a few seconds checking:

1. Maximum input size
2. Maximum value of each element
3. Arithmetic involving those values
4. Possible overflow
5. Maximum memory required
6. Whether recursion depth is safe

---

## Example: Coin Change

Constraints:

coins[i] <= 2^31 - 1
amount <= 10^4

If I calculate:

currSum + coins[index]

using Java `int`, overflow is possible.

Example:

currSum = 10
coin = 2^31 - 1

The mathematical result is:

2,147,483,657

which is larger than Integer.MAX_VALUE.

Therefore the Java `int` result overflows and becomes negative.

That can break:

if (currSum > target)

because the overflowed value may now look smaller than the target.

---

## Interview Habit

Before implementation, ask:

> "What is the largest value each variable can reach?"

Especially inspect:

- sums
- products
- multiplication of indices
- `mid = (low + high) / 2`
- counters
- sentinel values such as Integer.MAX_VALUE

Don't let implementation pressure make me skip this step.

---

## Important Principle

> A correct algorithm + unsafe implementation = wrong answer.

Understanding the algorithm is not the final step.

I also need to prove that the implementation is safe under the given constraints.