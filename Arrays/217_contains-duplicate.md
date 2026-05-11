# Contains Duplicate

## Problem Statement

Given an integer array `nums`, return `true` if any value appears at least twice in the array, and return `false` if every element is distinct.

---

# Example

## Example 1

```js
Input: [1, 2, 3, 1];
Output: true;
```

## Example 2

```js
Input: [1, 2, 3, 4];
Output: false;
```

## Example 3

```js
Input: [1, 1, 1, 3, 3, 4, 3, 2, 4, 2];
Output: true;
```

---

# 1. Brute Force Approach

## Idea

Compare every element with every other element.

If any two elements are equal, return `true`.

---

## Code

```js
var containsDuplicate = function (nums) {
  for (let i = 0; i < nums.length; i++) {
    for (let j = i + 1; j < nums.length; j++) {
      if (nums[i] === nums[j]) {
        return true;
      }
    }
  }

  return false;
};
```

---

## Complexity

### Time Complexity

```txt
O(n²)
```

### Space Complexity

```txt
O(1)
```

---

## Why It’s Brute Force

- Checks all possible pairs
- Nested loops
- Slow for large input sizes

---

# 2. Better Approach (Sorting)

## Idea

Sort the array.

After sorting:

- duplicate elements become adjacent
- compare neighboring elements

---

## Code

```js
var containsDuplicate = function (nums) {
  nums.sort((a, b) => a - b);

  for (let i = 1; i < nums.length; i++) {
    if (nums[i] === nums[i - 1]) {
      return true;
    }
  }

  return false;
};
```

---

## Complexity

### Time Complexity

```txt
O(n log n)
```

### Space Complexity

```txt
O(1) or O(n)
```

(depending on sorting implementation)

---

## Why Better?

- Faster than brute force
- Avoids nested comparison loops
- Uses sorting logic efficiently

---

# 3. Optimal Approach (HashSet)

## Idea

Use a `Set` for constant-time lookup.

While traversing:

- if element already exists in set → duplicate found
- otherwise add it to set

---

## Code

```js
var containsDuplicate = function (nums) {
  let set = new Set();

  for (let num of nums) {
    if (set.has(num)) {
      return true;
    }

    set.add(num);
  }

  return false;
};
```

---

## Complexity

### Time Complexity

```txt
O(n)
```

### Space Complexity

```txt
O(n)
```

---

## Why Optimal?

- Single traversal
- Constant-time lookup
- Best interview solution
- Very scalable for normal use cases

---

# Dry Run

## Input

```js
[1, 2, 3, 1];
```

---

## Flow

```txt
set = {}

1 → add
2 → add
3 → add
1 → already exists → true
```

---

# Interview Discussion

## ▶️ Why did you do it this way?

I used a `HashSet` because:

- lookup is fast
- `set.has()` works in approximately O(1)
- problem only requires existence checking

This allows solving the problem in one traversal.

---

## ▶️ What would you change if traffic grows 10x?

### Memory Optimization

Current approach uses:

```txt
O(n) space
```

If memory becomes constrained:

### Alternative → Sorting

```txt
Time: O(n log n)
Space: lower
```

Trade-off:

- slower execution
- lower memory usage

---

### Streaming Data Scenario

If data arrives continuously:

- HashSet still works well
- supports real-time duplicate detection

Examples:

- fraud detection
- log processing
- event streaming

---

### Distributed Scale

For massive datasets:

- distribute data across systems
- use Redis / Kafka / Spark
- Bloom filters for probabilistic detection

---

## ▶️ Can you debug this live?

### Bug Example

```js
if(set.get(num))
```

Issue:

`Set` does not have `.get()` method.

Correct:

```js
set.has(num);
```

---

### Another Common Bug

Forgetting:

```js
return false;
```

Then function returns:

```txt
undefined
```

---

## Live Debugging Steps

1. Reproduce issue
2. Use sample input
3. Add logs

```js
console.log(set, num);
```

4. Verify:

- insertion
- lookup
- loop execution
- return conditions

5. Test edge cases

---

## ▶️ What trade-offs did you make, and why?

### HashSet Approach

```txt
Time  → O(n)
Space → O(n)
```

Trade-off:

- extra memory
- faster execution

Why acceptable?

- interviews prioritize optimized runtime
- memory usage is manageable for most systems

---

### Sorting Trade-off

```txt
Time  → O(n log n)
Space → lower
```

Pros:

- lower memory

Cons:

- slower
- modifies original array

---

## ▶️ How did you test this, and what edge cases did you consider?

### Normal Cases

```js
[1,2,3,1] → true
[1,2,3,4] → false
```

---

## Edge Cases

### 1. Empty Array

```js
[] → false
```

---

### 2. Single Element

```js
[1] → false
```

---

### 3. All Duplicates

```js
[1,1,1,1] → true
```

---

### 4. Negative Numbers

```js
[-1,-2,-1] → true
```

---

### 5. Large Numbers

```js
[999999999, 999999999];
```

---

### 6. Duplicate at End

```js
[1, 2, 3, 4, 5, 1];
```

---

### 7. Already Sorted Input

```js
[1, 2, 3, 4];
```

---

# Final Interview Summary

“I used a HashSet because it provides O(1) lookup and solves the problem in one pass with O(n) time complexity. The main trade-off is additional memory usage, which is acceptable for interview-scale problems. If memory constraints become important at larger scale, sorting or distributed approaches can be considered depending on requirements.”

---

# Key Takeaways

- `Set` is best for duplicate detection
- Hashing is one of the most important DSA patterns
- Always discuss trade-offs in interviews
- Testing edge cases is critical
- Understand scalability beyond coding
