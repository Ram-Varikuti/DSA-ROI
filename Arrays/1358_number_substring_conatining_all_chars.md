# 1358. Number of Substrings Containing All Three Characters

## Problem Statement

Given a string `s` consisting only of characters:

```text
'a', 'b', 'c'
```

Return the number of substrings containing at least one occurrence of all three characters.

---

## Example

### Input

```js
s = "abcabc";
```

### Output

```js
10;
```

### Valid Substrings

```text
abc
abca
abcab
abcabc
bca
bcab
bcabc
cab
cabc
abc
```

---

# Approach 1: Brute Force

## Idea

Generate every substring and check whether it contains:

```text
a
b
c
```

Since the string contains only `a`, `b`, and `c`, if a set size becomes `3`, we know all characters are present.

---

## Code

```js
var numberOfSubstrings = function (s) {
  let count = 0;

  for (let i = 0; i < s.length; i++) {
    let set = new Set();

    for (let j = i; j < s.length; j++) {
      set.add(s[j]);

      if (set.size === 3) {
        count++;
      }
    }
  }

  return count;
};
```

---

## Dry Run

### Input

```js
s = "abc";
```

### i = 0

```js
j = 0;
set = { a };

j = 1;
set = { a, b };

j = 2;
set = { a, b, c };
size = 3;
count = 1;
```

### i = 1

```js
set = { b };
set = { b, c };
```

No valid substring.

### i = 2

```js
set = { c };
```

No valid substring.

### Result

```js
count = 1;
```

---

## Complexity

```text
Time  : O(n²)
Space : O(1)
```

---

# Approach 2: Sliding Window (Optimal)

## Key Observation

Whenever a window contains:

```text
a
b
c
```

every extension of that window to the right will also remain valid.

Example:

```text
abc
abca
abcab
abcabc
```

Instead of counting each substring one by one, count all of them together.

---

## Important Formula

```js
count += s.length - right;
```

---

## Why Does This Work?

Suppose:

```text
a b c a b c
0 1 2 3 4 5
    ^
  right = 2
```

Current window:

```text
abc
```

Already contains:

```text
a ✓
b ✓
c ✓
```

Possible valid endings:

```text
2
3
4
5
```

Substrings:

```text
abc
abca
abcab
abcabc
```

Count:

```js
6 - 2 = 4
```

Therefore:

```js
count += s.length - right;
```

---

# Algorithm

### Step 1

Expand window using `right`.

```js
freq[s[right]]++;
```

---

### Step 2

Check whether window contains all characters.

```js
freq["a"] > 0 && freq["b"] > 0 && freq["c"] > 0;
```

---

### Step 3

Count all valid substrings.

```js
count += s.length - right;
```

---

### Step 4

Shrink window.

```js
freq[s[left]]--;
left++;
```

Continue searching for more valid windows.

---

# Optimal Code

```js
var numberOfSubstrings = function (s) {
  let count = 0;
  let left = 0;

  let freq = {
    a: 0,
    b: 0,
    c: 0,
  };

  for (let right = 0; right < s.length; right++) {
    freq[s[right]]++;

    while (freq["a"] > 0 && freq["b"] > 0 && freq["c"] > 0) {
      count += s.length - right;

      freq[s[left]]--;
      left++;
    }
  }

  return count;
};
```

---

# Detailed Dry Run

### Input

```js
s = "abcabc";
```

```js
count = 0;
left = 0;

freq = {
  a: 0,
  b: 0,
  c: 0,
};
```

---

## right = 0

```js
freq = {
  a: 1,
  b: 0,
  c: 0,
};
```

Not valid.

```js
count = 0;
```

---

## right = 1

```js
freq = {
  a: 1,
  b: 1,
  c: 0,
};
```

Not valid.

```js
count = 0;
```

---

## right = 2

```js
freq = {
  a: 1,
  b: 1,
  c: 1,
};
```

Valid.

```js
count += 6 - 2;
count += 4;
```

Substrings:

```text
abc
abca
abcab
abcabc
```

```js
count = 4;
```

Shrink:

```js
freq[a]--;
left = 1;
```

---

## right = 3

Window:

```text
bca
```

Valid.

```js
count += 6 - 3;
count += 3;
```

Substrings:

```text
bca
bcab
bcabc
```

```js
count = 7;
```

Shrink.

---

## right = 4

Window:

```text
cab
```

Valid.

```js
count += 6 - 4;
count += 2;
```

Substrings:

```text
cab
cabc
```

```js
count = 9;
```

Shrink.

---

## right = 5

Window:

```text
abc
```

Valid.

```js
count += 6 - 5;
count += 1;
```

Substring:

```text
abc
```

```js
count = 10;
```

---

# Why Sliding Window Works

Once a window contains:

```text
a
b
c
```

adding more characters to the right cannot remove them.

So every larger window is automatically valid.

Instead of counting:

```text
abc
abca
abcab
abcabc
```

one by one, we count them together:

```js
count += s.length - right;
```

This optimization reduces the complexity from:

```text
O(n²)
```

to:

```text
O(n)
```

---

# Complexity Analysis

## Brute Force

```text
Time  : O(n²)
Space : O(1)
```

## Sliding Window

```text
Time  : O(n)
Space : O(1)
```

---

# Edge Cases

### Case 1

```js
s = "a";
```

Output:

```js
0;
```

---

### Case 2

```js
s = "aaaa";
```

Output:

```js
0;
```

---

### Case 3

```js
s = "aabb";
```

Output:

```js
0;
```

---

### Case 4

```js
s = "abc";
```

Output:

```js
1;
```

---

# Interview Explanation

> We use a sliding window and maintain the frequency of `a`, `b`, and `c`. Whenever the window contains all three characters, every substring formed by extending the right boundary to the end of the string will also be valid. Therefore, instead of counting them individually, we add `s.length - right` to the answer and shrink the window to find more valid starting positions. Since each character enters and leaves the window at most once, the solution runs in O(n) time.

---

# Pattern

```text
Sliding Window
Count Substrings
Frequency Map
At Least One Occurrence
```

### Recognition Hint

If the problem says:

- Count substrings
- Contains all required characters
- At least one occurrence of each

Think:

```text
Sliding Window + Count Valid Endings
```
