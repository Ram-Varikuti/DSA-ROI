# 560. Subarray Sum Equals K

## Problem Statement

Given an integer array `nums` and an integer `k`, return the **total number of continuous subarrays whose sum equals `k`**.

### Example

```javascript
Input: (nums = [1, 1, 1]), (k = 2);
Output: 2;
```

**Explanation**

```javascript
[1,1] => sum = 2
[1,1] => sum = 2
```

Total valid subarrays = **2**

---

# Approach 1: Brute Force

## Idea

Generate every possible subarray and calculate its sum.

If the sum equals `k`, increment the count.

---

## Algorithm

1. Start each subarray from index `i`.
2. Extend the subarray using index `j`.
3. Keep a running sum.
4. If running sum equals `k`, increment count.

---

## Code

```javascript
var subarraySum = function (nums, k) {
  let count = 0;

  for (let i = 0; i < nums.length; i++) {
    let sum = 0;

    for (let j = i; j < nums.length; j++) {
      sum += nums[j];

      if (sum === k) {
        count++;
      }
    }
  }

  return count;
};
```

---

## Complexity Analysis

| Metric | Complexity |
| ------ | ---------- |
| Time   | O(n²)      |
| Space  | O(1)       |

---

# Approach 2: Prefix Sum + HashMap (Optimal)

## Key Observation

For any subarray:

```text
Subarray Sum = Current Prefix Sum - Previous Prefix Sum
```

If:

```text
Current Prefix Sum - Previous Prefix Sum = k
```

Then:

```text
Previous Prefix Sum = Current Prefix Sum - k
```

So for every position, we only need to know:

> Have we seen a prefix sum equal to `(currentPrefixSum - k)` before?

If yes, then a valid subarray exists.

---

# What is Prefix Sum?

For:

```javascript
nums = [1, 2, 3, 4];
```

Prefix sums:

```text
Index      0  1  2  3
Value      1  2  3  4
Prefix     1  3  6  10
```

Where:

```text
1
1 + 2
1 + 2 + 3
1 + 2 + 3 + 4
```

---

# Why HashMap?

We store:

```text
prefixSum -> frequency
```

Example:

```javascript
{
    0 : 1,
    1 : 2,
    3 : 1
}
```

Frequency matters because the same prefix sum can occur multiple times.

Each occurrence creates another valid subarray.

---

# Why `map.set(0, 1)`?

This handles subarrays that start from index `0`.

### Example

```javascript
nums = [3];
k = 3;
```

When:

```javascript
prefixSum = 3;
```

We check:

```javascript
3 - 3 = 0
```

Since `0` already exists in the map:

```javascript
map.get(0) = 1
```

We correctly count the subarray.

Without:

```javascript
map.set(0, 1);
```

we would miss this case.

---

# Optimal Solution

```javascript
var subarraySum = function (nums, k) {
  let map = new Map();

  map.set(0, 1);

  let prefixSum = 0;
  let count = 0;

  for (let num of nums) {
    prefixSum += num;

    if (map.has(prefixSum - k)) {
      count += map.get(prefixSum - k);
    }

    map.set(prefixSum, (map.get(prefixSum) || 0) + 1);
  }

  return count;
};
```

---

# Dry Run

### Input

```javascript
nums = [1, 1, 1];
k = 2;
```

### Initial State

```javascript
map = {
  0: 1,
};

prefixSum = 0;
count = 0;
```

---

### Iteration 1

```javascript
num = 1;

prefixSum = 1;
```

Check:

```javascript
1 - 2 = -1
```

Not found.

Store:

```javascript
map = {
  0: 1,
  1: 1,
};
```

---

### Iteration 2

```javascript
num = 1;

prefixSum = 2;
```

Check:

```javascript
2 - 2 = 0
```

Found.

```javascript
count += 1;

count = 1;
```

Store:

```javascript
map = {
  0: 1,
  1: 1,
  2: 1,
};
```

---

### Iteration 3

```javascript
num = 1;

prefixSum = 3;
```

Check:

```javascript
3 - 2 = 1
```

Found.

```javascript
count += 1;

count = 2;
```

Store:

```javascript
map = {
  0: 1,
  1: 1,
  2: 1,
  3: 1,
};
```

---

### Final Answer

```javascript
count = 2;
```

---

# Visual Understanding

```text
nums = [1,1,1]

Index        -1   0   1   2
Prefix Sum    0   1   2   3
```

When:

```text
prefixSum = 2
```

Look for:

```text
2 - 2 = 0
```

Found.

Subarray:

```text
[1,1]
```

---

When:

```text
prefixSum = 3
```

Look for:

```text
3 - 2 = 1
```

Found.

Subarray:

```text
[1,1]
```

---

# Why Update Map After Checking?

Correct order:

```javascript
if (map.has(prefixSum - k)) {
  count += map.get(prefixSum - k);
}

map.set(prefixSum, (map.get(prefixSum) || 0) + 1);
```

If we update first, we may incorrectly count the current prefix sum as a previous prefix sum.

---

# Complexity Analysis

| Metric | Complexity |
| ------ | ---------- |
| Time   | O(n)       |
| Space  | O(n)       |

---

# Edge Cases

### Single Element

```javascript
nums = [3];
k = 3;

Output = 1;
```

---

### Negative Numbers

```javascript
nums = [1, -1, 0];
k = 0;

Output = 3;
```

Subarrays:

```javascript
[1, -1][0][(1, -1, 0)];
```

---

### No Valid Subarray

```javascript
nums = [1, 2, 3];
k = 7;

Output = 0;
```

---

### Multiple Prefix Sum Matches

```javascript
nums = [0, 0, 0];
k = 0;

Output = 6;
```

---

# Interview Notes

### Why did you use Prefix Sum + HashMap?

Brute force checks all subarrays and takes **O(n²)** time.

Using Prefix Sum + HashMap allows us to determine in constant time whether a previous prefix sum exists that forms a subarray sum of `k`, reducing the complexity to **O(n)**.

---

### What would you do if traffic grows 10x?

The algorithm is already optimal in terms of time complexity.

Further improvements would focus on:

- Memory optimization
- Distributed processing for huge datasets
- Efficient streaming approaches

---

### How would you debug this?

Track:

```javascript
prefixSum;
prefixSum - k;
count;
map;
```

Common mistakes:

- Forgetting `map.set(0, 1)`
- Updating the map before checking
- Using a Set instead of a frequency map

---

### Trade-Offs

| Approach             | Time  | Space |
| -------------------- | ----- | ----- |
| Brute Force          | O(n²) | O(1)  |
| Prefix Sum + HashMap | O(n)  | O(n)  |

We trade extra space for significantly faster execution.

---

# Pattern Recognition

Whenever you hear:

- Count subarrays
- Sum equals K
- Number of subarrays
- Continuous subarray sum
- Negative numbers exist

Think:

```text
Prefix Sum + HashMap Frequency Pattern
```

---

# Reusable Template

```javascript
let map = new Map();

map.set(0, 1);

let prefixSum = 0;
let answer = 0;

for (let num of nums) {
  prefixSum += num;

  if (map.has(prefixSum - target)) {
    answer += map.get(prefixSum - target);
  }

  map.set(prefixSum, (map.get(prefixSum) || 0) + 1);
}

return answer;
```

---

# Similar Problems

- 930. Binary Subarrays With Sum
- 974. Subarray Sums Divisible by K
- 1248. Count Number of Nice Subarrays
- 525. Contiguous Array
- 523. Continuous Subarray Sum

### Golden Formula

```text
currentPrefixSum - previousPrefixSum = k
```

Rearrange:

```text
previousPrefixSum = currentPrefixSum - k
```

This single formula is the foundation of the entire Prefix Sum + HashMap pattern. 🚀
