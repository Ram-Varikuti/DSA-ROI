# Top K Frequent Words — README Notes

## Problem Statement

Given an array of strings `words` and an integer `k`,
return the `k` most frequent words.

### Rules

- Higher frequency comes first
- If frequencies are same → alphabetical order

---

# Example

```js
Input: words = ["i", "love", "leetcode", "i", "love", "coding"];
k = 2;

Output: ["i", "love"];
```

---

# Approach 1 — Brute Force

## Idea

For every word:

- Count frequency manually
- Repeatedly find highest frequency word

---

## Code

```js
var topKFrequent = function (words, k) {
  let visited = new Set();
  let result = [];

  while (result.length < k) {
    let bestWord = "";
    let bestFreq = 0;

    for (let i = 0; i < words.length; i++) {
      let word = words[i];

      if (visited.has(word)) continue;

      // Count frequency manually
      let freq = 0;

      for (let j = 0; j < words.length; j++) {
        if (words[j] === word) {
          freq++;
        }
      }

      // Better candidate
      if (freq > bestFreq || (freq === bestFreq && word < bestWord)) {
        bestFreq = freq;
        bestWord = word;
      }
    }

    visited.add(bestWord);
    result.push(bestWord);
  }

  return result;
};
```

---

# Complexity

## Time Complexity

```text
O(n²)
```

## Space Complexity

```text
O(k)
```

---

# Why Brute Force is Bad

We repeatedly count frequencies again and again.

Huge duplicate work.

---

# Approach 2 — Better (HashMap + Sorting)

This is the most common interview solution.

---

# Step 1 → Frequency Map

```js
{
  i: 2,
  love: 2,
  coding: 1
}
```

---

# Step 2 → Convert to Array

```js
Object.entries(freqMap);
```

becomes:

```js
[
  ["i", 2],
  ["love", 2],
  ["coding", 1],
];
```

Structure:

```js
[word, frequency];
```

---

# Step 3 → Custom Sorting

```js
arr.sort((a, b) => {
  // Higher frequency first
  if (b[1] !== a[1]) {
    return b[1] - a[1];
  }

  // Alphabetical order
  return a[0].localeCompare(b[0]);
});
```

---

# Deep Understanding of Comparator

---

# Meaning of a and b

```js
a = ["coding", 1];
b = ["i", 2];
```

Then:

```js
a[0] = word;
a[1] = frequency;
```

---

# Frequency Sorting

```js
return b[1] - a[1];
```

Means:

```text
Higher frequency first
```

---

# Example

```js
2 - 1 = 1
```

Positive means:

```text
b comes first
```

So higher frequency moves ahead.

---

# Same Frequency

```js
return a[0].localeCompare(b[0]);
```

Used for alphabetical ordering.

---

# Example

```js
"i".localeCompare("love");
```

Returns negative.

Meaning:

```text
i comes first
```

---

# Better Solution Code

```js
var topKFrequent = function (words, k) {
  // Step 1
  let freqMap = {};

  for (let word of words) {
    freqMap[word] = (freqMap[word] || 0) + 1;
  }

  // Step 2
  let arr = Object.entries(freqMap);

  // Step 3
  arr.sort((a, b) => {
    if (b[1] !== a[1]) {
      return b[1] - a[1];
    }

    return a[0].localeCompare(b[0]);
  });

  // Step 4
  let result = [];

  for (let i = 0; i < k; i++) {
    result.push(arr[i][0]);
  }

  return result;
};
```

---

# Complexity

## Time Complexity

```text
O(m log m)
```

where:

- `m = unique words`

---

## Space Complexity

```text
O(m)
```

---

# Approach 3 — Optimal (Min Heap)

Best for:

- Huge datasets
- Very small `k`

---

# Core Idea

Instead of sorting ALL words:

- Maintain only top `k` words

---

# Steps

## Step 1

Build frequency map.

---

## Step 2

Push words into min heap.

If heap size exceeds `k`:

- Remove smallest frequency word.

---

# Heap Ordering

Priority:

1. Smaller frequency first
2. Same frequency → lexicographically larger removed first

---

# Complexity

## Time Complexity

```text
O(m log k)
```

Better when:

```text
k << m
```

---

## Space Complexity

```text
O(m)
```

---

# Approach Comparison

| Approach    | Time       | Space | Notes                   |
| ----------- | ---------- | ----- | ----------------------- |
| Brute Force | O(n²)      | O(k)  | Very slow               |
| Sorting     | O(m log m) | O(m)  | Best interview solution |
| Heap        | O(m log k) | O(m)  | Best scalable solution  |

---

# Important Concepts

---

# 1. Frequency Counting

```js
freqMap[word] = (freqMap[word] || 0) + 1;
```

---

# 2. Object.entries()

Converts object into array.

```js
{
  i: 2;
}
```

becomes:

```js
[["i", 2]];
```

---

# 3. Number Sorting

## Ascending

```js
a - b;
```

---

## Descending

```js
b - a;
```

---

# 4. String Sorting

Use:

```js
a.localeCompare(b);
```

---

# Simple Dry Run

Input:

```js
[
  ["love", 2],
  ["i", 2],
  ["coding", 1],
];
```

---

# Compare love vs i

Same frequency:

```js
"love".localeCompare("i");
```

Positive:

```text
i comes first
```

---

# Compare coding vs love

```js
2 - 1 = 1
```

Positive:

```text
love comes first
```

---

# Final Array

```js
[
  ["i", 2],
  ["love", 2],
  ["coding", 1],
];
```

---

# Interview Questions

---

# ▶️ Why HashMap?

Fast frequency counting.

```text
O(1)
```

average insertion.

---

# ▶️ Why localeCompare?

Strings cannot use subtraction.

Use:

```js
a.localeCompare(b);
```

for alphabetical ordering.

---

# ▶️ Why Heap Better for Large Data?

Heap keeps only:

- top `k` elements

Avoids sorting everything.

---

# ▶️ Trade-offs

| Method  | Pros     | Cons                  |
| ------- | -------- | --------------------- |
| Sorting | Easy     | Sorts everything      |
| Heap    | Scalable | Harder implementation |

---

# Edge Cases

## Single Word

```js
["a"], (k = 1);
```

---

## Same Frequencies

```js
["a", "b", "c"];
```

Alphabetical order matters.

---

## All Same Words

```js
["x", "x", "x"];
```

---

# Pattern Recognition

This problem teaches:

- HashMap
- Frequency Counting
- Custom Comparator
- Multi-level Sorting
- Heap
- Top K Pattern

Related:

- Top K Frequent Elements
- Sort Characters By Frequency
- K Closest Points to Origin
