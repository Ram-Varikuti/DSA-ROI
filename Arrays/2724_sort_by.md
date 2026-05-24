# Sort Array By Function Output

## Problem Statement

Given:

- an array `arr`
- a function `fn`

Return the array sorted in ascending order based on the value returned by `fn`.

You may assume:

- `fn()` always returns numbers
- no duplicate outputs from `fn`

---

# Solution

```js
var sortBy = function (arr, fn) {
  return arr.sort((a, b) => fn(a) - fn(b));
};
```

---

# Core Idea

We use JavaScript’s built-in:

```js
sort();
```

But instead of normal sorting, we provide a **custom comparator function**.

---

# Comparator Logic

```js
(a, b) => fn(a) - fn(b);
```

This means:

- compare `fn(a)` and `fn(b)`
- smaller result should come first

---

# How `sort()` Works

## Rule 1 → Negative Result

```js
fn(a) - fn(b) < 0;
```

→ `a` stays before `b`

---

## Rule 2 → Positive Result

```js
fn(a) - fn(b) > 0;
```

→ `b` moves before `a`

---

## Rule 3 → Zero

```js
fn(a) - fn(b) === 0;
```

→ order remains unchanged

---

# Dry Run

## Input

```js
arr = [5, 2, 4, 1, 3];

fn = (x) => x;
```

---

## Step 1

Compare:

```js
5 and 2
```

Calculation:

```js
fn(5) - fn(2)
= 5 - 2
= 3
```

Positive → move `2` before `5`

Array:

```js
[2, 5, 4, 1, 3];
```

---

## Step 2

Compare:

```js
5 and 4
```

```js
5 - 4 = 1
```

Positive → move `4` before `5`

Array:

```js
[2, 4, 5, 1, 3];
```

---

## Step 3

Compare:

```js
5 and 1
```

```js
5 - 1 = 4
```

Positive → move `1` before `5`

Array:

```js
[2, 4, 1, 5, 3];
```

---

JavaScript continues comparisons until array becomes:

```js
[1, 2, 3, 4, 5];
```

---

# Example 2

## Input

```js
arr = ["apple", "kiwi", "banana"];

fn = (x) => x.length;
```

---

## Function Output

| Element | fn(x) |
| ------- | ----- |
| apple   | 5     |
| kiwi    | 4     |
| banana  | 6     |

---

## Sorted Array

```js
["kiwi", "apple", "banana"];
```

Because:

```txt
4 < 5 < 6
```

---

# Why This Works

The comparator converts each element into a sortable numeric value using:

```js
fn(element);
```

Then sorting happens based on those values.

---

# Time Complexity

## Sorting Complexity

```txt
O(n log n)
```

---

# Space Complexity

```txt
O(1)
```

(ignoring internal sorting implementation)

---

# Interview Explanation

## Why use custom comparator?

Because default sorting does not know how we want to order elements.

---

## Why subtract?

```js
fn(a) - fn(b);
```

This is the shortest way to:

- place smaller values first
- create ascending order

---

# Common Pattern

```js
arr.sort((a, b) => condition);
```

Used in:

- custom sorting
- intervals
- objects
- greedy problems
- scheduling
- ranking systems

---

# General Template

```js
arr.sort((a, b) => {
  return valueA - valueB;
});
```

---

# Key Takeaway

Whenever sorting depends on:

- object properties
- string length
- frequencies
- custom calculations

use:

```js
sort((a, b) => ...)
```

This is one of the most important JavaScript interview patterns.
