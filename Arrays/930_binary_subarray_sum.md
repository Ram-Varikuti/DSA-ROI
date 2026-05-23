# 📘 930. Binary Subarrays With Sum

## 🔹 Problem Statement

Given a binary array `nums` and an integer `goal`,
return the number of non-empty subarrays whose sum equals `goal`.

---

# ✅ Example

```js
Input: nums = [1, 0, 1, 0, 1];
goal = 2;

Output: 4;
```

---

# 🔹 Core Idea → Prefix Sum + HashMap

Instead of checking every subarray (`O(n²)`),
we use:

- Prefix Sum
- HashMap

to solve it in:

```js
O(n);
```

---

# 📘 What is Prefix Sum?

Prefix sum means:

```js
running total from the beginning
```

---

## Example

```js
nums = [1, 0, 1, 0, 1];
```

| Index | Prefix Sum |
| ----- | ---------- |
| 0     | 1          |
| 1     | 1          |
| 2     | 2          |
| 3     | 2          |
| 4     | 3          |

---

# 📘 Important Formula

To calculate subarray sum:

```js
subarraySum = currentPrefixSum - previousPrefixSum;
```

Rearrange:

```js
previousPrefixSum = currentPrefixSum - goal;
```

This is the key observation.

---

# 📘 Why HashMap?

We need to quickly know:

```js
Has (sum - goal) appeared before?
```

So we store:

```js
prefixSum -> frequency
```

Example:

```js
{
   0 : 1,
   1 : 2,
   2 : 1
}
```

Means:

- Prefix sum `1` appeared 2 times.

---

# ✅ Optimal Solution

```js
function numSubarraysWithSum(nums, goal) {
  let map = new Map();

  // Base case
  map.set(0, 1);

  let sum = 0;
  let count = 0;

  for (let num of nums) {
    // Running prefix sum
    sum += num;

    // Check if valid prefix exists
    if (map.has(sum - goal)) {
      count += map.get(sum - goal);
    }

    // Store current prefix sum frequency
    map.set(sum, (map.get(sum) || 0) + 1);
  }

  return count;
}
```

---

# 📘 Simple Dry Run

```js
nums = [1, 0, 1, 0, 1];
goal = 2;
```

---

## 🔹 Initial State

```js
map = { 0: 1 };

sum = 0;
count = 0;
```

---

## 🔹 Step 1

```js
num = 1;

sum = 1;
```

Check:

```js
1 - 2 = -1
```

Not found.

Store:

```js
map = {
  0: 1,
  1: 1,
};
```

---

## 🔹 Step 2

```js
num = 0;

sum = 1;
```

Check:

```js
1 - 2 = -1
```

Not found.

Update:

```js
map = {
  0: 1,
  1: 2,
};
```

---

## 🔹 Step 3

```js
num = 1;

sum = 2;
```

Check:

```js
2 - 2 = 0
```

Found once.

```js
count = 1;
```

---

## 🔹 Step 4

```js
num = 0;

sum = 2;
```

Check:

```js
2 - 2 = 0
```

Found again.

```js
count = 2;
```

---

## 🔹 Step 5

```js
num = 1;

sum = 3;
```

Check:

```js
3 - 2 = 1
```

`1` appeared twice.

```js
count += 2;

count = 4;
```

---

# ✅ Final Answer

```js
4;
```

---

# 📘 Why `map.set(0,1)`?

Very important base case.

Means:

```js
prefix sum 0 already exists once
```

This helps count subarrays starting from index `0`.

---

## Example

```js
nums = [1, 1];
goal = 2;
```

At second index:

```js
sum = 2

sum - goal = 0
```

We need `0` already in map.

---

# 📘 Time Complexity

```js
O(n);
```

Single traversal.

---

# 📘 Space Complexity

```js
O(n);
```

For hashmap storage.

---

# 📘 Why Sliding Window Doesn't Work Properly?

Sliding window works best when numbers are positive and sum changes predictably.

Here zeros exist:

```js
[1, 0, 0, 1];
```

Removing left pointer may not reduce sum.

So prefix sum + hashmap is the safest and optimal approach.

---

# 📘 Interview Explanation

## ▶️ Why did you use this approach?

Brute force checks every subarray:

```js
O(n²)
```

Using prefix sum + hashmap reduces it to:

```js
O(n);
```

---

## ▶️ What happens if traffic grows 10x?

Current solution already scales well because:

- Single traversal
- Constant-time hashmap lookup
- No nested loops

---

## ▶️ Can you debug this live?

Main debugging areas:

1. Prefix sum updates
2. `(sum - goal)` calculation
3. HashMap frequencies
4. Base case:

```js
map.set(0, 1);
```

---

## ▶️ Trade-offs

Used extra hashmap memory:

```js
O(n);
```

to achieve:

```js
O(n);
```

time complexity.

---

# 📘 Edge Cases Tested

## ✅ Normal Case

```js
[1, 0, 1, 0, 1];
```

---

## ✅ All Zeros

```js
[0, 0, 0, 0];
```

---

## ✅ Single Element

```js
[1];
```

---

## ✅ No Valid Subarray

```js
[1, 1, 1];
```

---

## ✅ Entire Array Valid

```js
[1, 1, 0];
```
