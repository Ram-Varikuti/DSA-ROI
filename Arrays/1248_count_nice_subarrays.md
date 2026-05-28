# Count Number of Nice Subarrays

## Problem Statement

Given an array of integers `nums` and an integer `k`.

A continuous subarray is called **nice** if there are exactly `k` odd numbers in it.

Return the number of nice subarrays.

---

# Example

```js
Input: nums = [1, 1, 2, 1, 1];
k = 3;

Output: 2;
```

---

# Brute Force Approach

## Idea

- Generate all possible subarrays
- Count odd numbers in each subarray
- If odd count becomes `k`, increment answer

---

# Brute Force Code

```js
var numberOfSubarrays = function (nums, k) {
  let count = 0;

  for (let i = 0; i < nums.length; i++) {
    let odd = 0;

    for (let j = i; j < nums.length; j++) {
      if (nums[j] % 2 !== 0) {
        odd++;
      }

      if (odd === k) {
        count++;
      }
    }
  }

  return count;
};
```

---

# Brute Force Dry Run

## Input

```js
nums = [1, 1, 2, 1];
k = 2;
```

---

## Valid Subarrays

```js
[1, 1][(1, 1, 2)][(1, 2, 1)];
```

Answer:

```js
3;
```

---

# Brute Force Complexity

| Complexity | Value |
| ---------- | ----- |
| Time       | O(n²) |
| Space      | O(1)  |

---

# Optimal Approach (Prefix Sum + HashMap)

---

# Core Idea

We maintain:

```js
oddCount;
```

Meaning:

> Number of odd numbers seen so far.

---

# Important Observation

If:

```js
currentOddCount - previousOddCount = k
```

Then the subarray between them has exactly `k` odd numbers.

So we search for:

```js
previousOddCount = currentOddCount - k;
```

using hashmap.

---

# Optimal Code

```js
var numberOfSubarrays = function (nums, k) {
  let map = new Map();

  map.set(0, 1);

  let oddCount = 0;
  let result = 0;

  for (let num of nums) {
    if (num % 2 !== 0) {
      oddCount++;
    }

    if (map.has(oddCount - k)) {
      result += map.get(oddCount - k);
    }

    map.set(oddCount, (map.get(oddCount) || 0) + 1);
  }

  return result;
};
```

---

# Why `map.set(0,1)` ?

```js
map.set(0, 1);
```

means:

> Before starting, 0 odd numbers occurred once.

This helps handle subarrays starting from index `0`.

---

# Optimal Dry Run

## Input

```js
nums = [1, 1, 2, 1];
k = 2;
```

---

# Initial State

```js
map = { 0: 1 };

oddCount = 0;
result = 0;
```

---

# First Number = 1

```js
oddCount = 1;
```

Need:

```js
1 - 2 = -1
```

Not found.

Update map:

```js
map = {
  0: 1,
  1: 1,
};
```

---

# Second Number = 1

```js
oddCount = 2;
```

Need:

```js
2 - 2 = 0
```

Found.

```js
result = 1;
```

Subarray:

```js
[1, 1];
```

---

# Third Number = 2

Even number.

```js
oddCount = 2;
```

Need:

```js
2 - 2 = 0
```

Found again.

```js
result = 2;
```

Subarray:

```js
[1, 1, 2];
```

---

# Fourth Number = 1

```js
oddCount = 3;
```

Need:

```js
3 - 2 = 1
```

Found.

```js
result = 3;
```

Subarray:

```js
[1, 2, 1];
```

---

# Final Answer

```js
3;
```

---

# Complexity

| Complexity | Value |
| ---------- | ----- |
| Time       | O(n)  |
| Space      | O(n)  |

---

# Most Important Line

```js
result += map.get(oddCount - k);
```

Meaning:

> “How many previous times did we have `oddCount-k` odd numbers?”

Those positions help form valid subarrays.

---

# Pattern Used

## Prefix Sum + HashMap

This pattern is commonly used in:

- Subarray Sum Equals K
- Binary Subarrays With Sum
- Count Nice Subarrays

---

# Interview Explanation

> I maintain a running count of odd numbers.
>
> If previously I had `currentOddCount - k` odd numbers, then the subarray between those positions contains exactly `k` odd numbers.
>
> I use a hashmap to store frequencies and solve the problem in O(n).
