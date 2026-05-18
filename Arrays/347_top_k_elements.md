# Top K Frequent Elements — Complete README

## Problem Statement

Given an integer array `nums` and an integer `k`, return the `k` most frequent elements.

You may return the answer in any order.

---

# Example

## Input

```javascript
nums = [1, 1, 1, 2, 2, 3];
k = 2;
```

## Output

```javascript
[1, 2];
```

## Explanation

```text
1 appears 3 times
2 appears 2 times
3 appears 1 time
```

Top 2 frequent elements are:

```text
1 and 2
```

---

# Approach 1 — Sorting

---

# Idea

1. Count frequency using HashMap
2. Convert map into array
3. Sort by frequency descending
4. Return first `k` elements

---

# Code

```javascript
var topKFrequent = function (nums, k) {
  let map = {};

  // Count frequency
  for (let num of nums) {
    map[num] = (map[num] || 0) + 1;
  }

  // Convert to array
  let arr = Object.entries(map);

  // Sort descending by frequency
  arr.sort((a, b) => b[1] - a[1]);

  // Return first k elements
  return arr.slice(0, k).map((x) => Number(x[0]));
};
```

---

# Dry Run

## Frequency Map

```javascript
{
 1:3,
 2:2,
 3:1
}
```

---

## Array Conversion

```javascript
[
  ["1", 3],
  ["2", 2],
  ["3", 1],
];
```

---

## After Sorting

```javascript
[
  ["1", 3],
  ["2", 2],
  ["3", 1],
];
```

---

## First k Elements

```javascript
[1, 2];
```

---

# Complexity

## Time Complexity

```text
O(n log n)
```

## Space Complexity

```text
O(n)
```

---

# Approach 2 — Max Heap

---

# Idea

1. Count frequencies
2. Push all elements into Max Heap
3. Extract top `k` frequent elements

---

# Code

```javascript
var topKFrequent = function (nums, k) {
  let map = {};

  // Frequency count
  for (let num of nums) {
    map[num] = (map[num] || 0) + 1;
  }

  // Max Heap
  let pq = new MaxPriorityQueue((x) => x.freq);

  // Push all elements
  for (const key in map) {
    pq.push({
      val: Number(key),
      freq: map[key],
    });
  }

  let result = [];

  // Extract top k
  while (k--) {
    result.push(pq.pop().val);
  }

  return result;
};
```

---

# Dry Run

## Heap After Insertion

```text
[
 {1,3},
 {2,2},
 {3,1}
]
```

---

## Pop 1

```text
{1,3}
```

---

## Pop 2

```text
{2,2}
```

---

## Result

```javascript
[1, 2];
```

---

# Complexity

## Time Complexity

```text
O(n log n)
```

## Space Complexity

```text
O(n)
```

---

# Approach 3 — Min Heap (Best Interview Solution)

---

# Idea

1. Count frequencies
2. Keep only top `k` elements in Min Heap
3. Remove smallest frequency whenever heap size exceeds `k`

---

# Why Min Heap?

Min Heap keeps the smallest frequency at the top.

When heap size exceeds `k`,
remove the smallest frequency.

Thus heap always stores only top `k` frequent elements.

---

# Code

```javascript
var topKFrequent = function (nums, k) {
  let map = {};

  // Frequency count
  for (let num of nums) {
    map[num] = (map[num] || 0) + 1;
  }

  // Min Heap
  let pq = new MinPriorityQueue((x) => x.freq);

  for (const key in map) {
    pq.push({
      val: key,
      freq: map[key],
    });

    // Keep heap size fixed
    if (pq.size() > k) {
      pq.pop();
    }
  }

  return pq.toArray().map((x) => Number(x.val));
};
```

---

# Main Trick

```javascript
if (pq.size() > k) {
  pq.pop();
}
```

This keeps heap size fixed at `k`.

---

# Dry Run

## Frequency Map

```javascript
{
 1:3,
 2:2,
 3:1
}
```

---

## Insert 1

```text
[
 {1,3}
]
```

---

## Insert 2

```text
[
 {2,2},
 {1,3}
]
```

---

## Insert 3

```text
[
 {3,1},
 {1,3},
 {2,2}
]
```

Heap size becomes 3.

Remove smallest:

```text
{3,1}
```

Remaining:

```text
[
 {2,2},
 {1,3}
]
```

---

# Complexity

## Time Complexity

```text
O(n log k)
```

## Space Complexity

```text
O(n)
```

---

# Common Mistake in Min Heap

---

# Wrong

```javascript
for (const key in map) {
  pq.push({
    val: key,
    freq: map[key],
  });
}

if (pq.size() > k) {
  pq.pop();
}
```

---

# Why Wrong?

Heap grows to size:

```text
n
```

Only one element removed.

Optimization is lost.

---

# Correct

```javascript
for (const key in map) {
  pq.push({
    val: key,
    freq: map[key],
  });

  if (pq.size() > k) {
    pq.pop();
  }
}
```

---

# Important Pattern

This is called:

```text
Fixed Size Min Heap
```

Used in:

- Top K Frequent Elements
- K Largest Elements
- K Closest Points
- Kth Largest Element

---

# Approach 4 — Bucket Sort (Best Complexity)

---

# Idea

Maximum frequency can never exceed:

```text
nums.length
```

Create buckets:

```text
bucket[frequency] = elements
```

Traverse buckets backward.

---

# Visualization

## Frequency Map

```javascript
1 -> 3
2 -> 2
3 -> 1
```

---

## Buckets

```javascript
bucket[1] = [3];
bucket[2] = [2];
bucket[3] = [1];
```

Traverse from back:

```text
3 -> [1]
2 -> [2]
```

Result:

```javascript
[1, 2];
```

---

# Code

```javascript
var topKFrequent = function (nums, k) {
  let map = {};

  // Frequency count
  for (let num of nums) {
    map[num] = (map[num] || 0) + 1;
  }

  // Create buckets
  let bucket = Array(nums.length + 1)
    .fill()
    .map(() => []);

  // Fill buckets
  for (const key in map) {
    let freq = map[key];

    bucket[freq].push(Number(key));
  }

  let result = [];

  // Traverse backwards
  for (let i = bucket.length - 1; i >= 0; i--) {
    for (const num of bucket[i]) {
      result.push(num);

      if (result.length === k) {
        return result;
      }
    }
  }
};
```

---

# Complexity

## Time Complexity

```text
O(n)
```

## Space Complexity

```text
O(n)
```

---

# Comparison Table

| Approach    | Time Complexity | Space Complexity | Best For                    |
| ----------- | --------------- | ---------------- | --------------------------- |
| Sorting     | `O(n log n)`    | `O(n)`           | Beginner                    |
| Max Heap    | `O(n log n)`    | `O(n)`           | Heap practice               |
| Min Heap    | `O(n log k)`    | `O(n)`           | Best interview solution     |
| Bucket Sort | `O(n)`          | `O(n)`           | Best theoretical complexity |

---

# Which Approach Should You Use?

## Beginner Level

Use:

```text
Sorting
```

---

## Interview Level

Use:

```text
Min Heap
```

Most expected solution.

---

## Advanced / FAANG Optimization

Use:

```text
Bucket Sort
```

Best time complexity.

---

# Key Interview Takeaways

✅ Always start with brute force

✅ Then optimize using Heap

✅ Mention Bucket Sort for best complexity

✅ Fixed-size Min Heap is a very important pattern

✅ Frequency counting is a very common interview technique

---

# Similar Problems

- Kth Largest Element
- K Closest Points to Origin
- Top K Frequent Words
- Find K Weakest Rows
- Merge K Sorted Lists
- Kth Smallest Element

---

# Patterns Learned

## 1. Frequency Counting

```javascript
map[num] = (map[num] || 0) + 1;
```

---

## 2. Fixed Size Min Heap

```javascript
push;

if (heap.size() > k) {
  pop;
}
```

---

## 3. Bucket Sort

```text
Index = frequency
Value = elements
```
