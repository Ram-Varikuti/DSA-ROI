# 2395. Find Subarrays With Equal Sum

---

# Problem Statement

Given an integer array `nums`, determine whether there exist **two subarrays of length 2** having the **same sum**.

Return:

- `true` → if duplicate pair sums exist
- `false` → otherwise

---

# Example

## Input

```js
nums = [4, 2, 4];
```

## Adjacent Pair Sums

```js
[4,2] = 6
[2,4] = 6
```

Duplicate sum found.

## Output

```js
true;
```

---

# Key Observation

We only need to calculate sums of adjacent pairs:

```js
nums[i] + nums[i + 1];
```

If the same sum appears again, return `true`.

---

# Optimal Approach → HashSet

Use a `Set` to store already seen sums.

For every adjacent pair:

1. Calculate pair sum
2. Check if sum already exists
3. If yes → return `true`
4. Otherwise store it in Set

---

# Algorithm

```text
Create empty Set

Loop from i = 0 to n-2

    sum = nums[i] + nums[i+1]

    If sum already exists
        return true

    Store sum in Set

Return false
```

---

# JavaScript Solution

```js
function findSubarrays(nums) {
  let seen = new Set();

  for (let i = 0; i < nums.length - 1; i++) {
    let sum = nums[i] + nums[i + 1];

    // Duplicate sum found
    if (seen.has(sum)) {
      return true;
    }

    // Store current sum
    seen.add(sum);
  }

  return false;
}
```

---

# Dry Run

## Input

```js
nums = [1, 2, 3, 1, 2];
```

---

## Iteration 1

```js
sum = 1 + 2 = 3
```

Set:

```js
{
  3;
}
```

---

## Iteration 2

```js
sum = 2 + 3 = 5
```

Set:

```js
{
  3, 5;
}
```

---

## Iteration 3

```js
sum = 3 + 1 = 4
```

Set:

```js
{
  3, 5, 4;
}
```

---

## Iteration 4

```js
sum = 1 + 2 = 3
```

Already exists in Set.

Return:

```js
true;
```

---

# Complexity Analysis

## Time Complexity

```text
O(n)
```

Single traversal of array.

---

## Space Complexity

```text
O(n)
```

For storing pair sums inside Set.

---

# Why Set?

Set gives:

```text
O(1)
```

average lookup time.

Perfect for:

- Duplicate detection
- Fast searching
- Presence checking

---

# Edge Cases

## Case 1

```js
nums = [1, 2];
```

Only one pair exists.

Output:

```js
false;
```

---

## Case 2

```js
nums = [0, 0, 0];
```

Pair sums:

```js
0;
0;
```

Duplicate found.

Output:

```js
true;
```

---

## Case 3

```js
nums = [-1, -2, -3];
```

Pair sums:

```js
-3 - 5;
```

No duplicate.

Output:

```js
false;
```

---

# Interview Explanation

## Why did you choose Set?

Because duplicate checking becomes efficient.

Without Set:

```text
O(n²)
```

With Set:

```text
O(n)
```

---

## What would you change if traffic grows 10x?

Current solution is already optimal:

- Single pass
- Constant lookup
- Minimal logic

No major optimization needed.

---

## Can you debug this live?

Yes.

Print:

```js
console.log(i, sum, seen);
```

This helps track:

- current index
- current sum
- Set contents

---

## Trade-offs

### Using Set

✅ Fast lookup
✅ Simple implementation
❌ Extra memory usage

---

## Alternative Approach

Using nested loops:

```text
O(n²)
```

Not efficient.

---

# Pattern Used

## Hashing / Set Pattern

Used when problem asks:

```text
"Have we seen this before?"
```

Common in:

- Contains Duplicate
- Two Sum
- Happy Number
- Longest Consecutive Sequence
- Subarray problems

---

# Key Takeaway

Whenever duplicate detection is required:

```text
Think about Set or HashMap immediately.
```

They reduce lookup time drastically and simplify the solution.
