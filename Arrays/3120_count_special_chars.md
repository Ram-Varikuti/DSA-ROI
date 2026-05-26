# 3120. Count the Number of Special Characters I

## Problem

A letter is called **special** if:

- it appears in lowercase
- and also appears in uppercase

Return the count of special letters.

---

# Example

```js
Input: word = "aaAbBc";

Output: 2;
```

Explanation:

```js
a and A → special
b and B → special
c only lowercase → not special
```

---

# Approach

We use a `Set` to store all characters.

Then:

- traverse characters
- check only lowercase letters
- verify whether uppercase version exists

If yes → increase count.

---

# Code

```js
var numberOfSpecialChars = function (word) {
  let set = new Set(word);
  let count = 0;

  for (let ch of set) {
    // Process only lowercase letters
    if (ch >= "a" && ch <= "z") {
      // Check uppercase exists
      if (set.has(ch.toUpperCase())) {
        count++;
      }
    }
  }

  return count;
};
```

---

# Key Concept

## `toUpperCase()`

Converts lowercase letter into uppercase.

```js
"a".toUpperCase(); // 'A'
"b".toUpperCase(); // 'B'
```

---

# Dry Run

## Input

```js
word = "aaAbBc";
```

## Step 1 → Create Set

```js
set = { 'a', 'A', 'b', 'B', 'c' }
```

Duplicates removed automatically.

---

## Step 2 → Traverse Set

### ch = 'a'

```js
'A' exists
count = 1
```

---

### ch = 'A'

Skip because uppercase.

---

### ch = 'b'

```js
'B' exists
count = 2
```

---

### ch = 'B'

Skip.

---

### ch = 'c'

```js
'C' not exists
```

---

# Final Answer

```js
2;
```

---

# Time Complexity

```js
O(n);
```

- traversing string/set once

---

# Space Complexity

```js
O(n);
```

- storing characters in set

---

# Important Interview Points

## Why use Set?

Set gives:

```js
O(1);
```

lookup time.

Efficient for checking existence.

---

# Pattern Used

## Hashing / Set Pattern

Used when:

- checking existence
- removing duplicates
- fast lookup required

---

# Similar Problems

- Contains Duplicate
- Happy Number
- Longest Consecutive Sequence
- Unique Characters
