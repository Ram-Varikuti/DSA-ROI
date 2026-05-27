# Balanced Number

## Problem Statement

Given a string `num` containing digits:

Return:

- `true` → if sum of digits at even indices equals sum of digits at odd indices
- `false` → otherwise

---

# Example

```js
Input: "1230"

Even indices:
0 → 1
2 → 3

Even Sum = 1 + 3 = 4

Odd indices:
1 → 2
3 → 0

Odd Sum = 2 + 0 = 2

Output: false
```

---

# Approach 1 — Brute Force

## Idea

- Store even index digits separately
- Store odd index digits separately
- Calculate sums later

---

## Code

```js
var isBalanced = function (num) {
  let evenArr = [];
  let oddArr = [];

  for (let i = 0; i < num.length; i++) {
    if (i % 2 === 0) {
      evenArr.push(Number(num[i]));
    } else {
      oddArr.push(Number(num[i]));
    }
  }

  let evenSum = 0;
  let oddSum = 0;

  for (let val of evenArr) {
    evenSum += val;
  }

  for (let val of oddArr) {
    oddSum += val;
  }

  return evenSum === oddSum;
};
```

---

## Complexity

### Time Complexity

```text
O(n)
```

### Space Complexity

```text
O(n)
```

---

# Approach 2 — Better

## Idea

Instead of storing arrays:

- Directly calculate sums during traversal

This reduces extra space usage.

---

## Code

```js
var isBalanced = function (num) {
  let even = 0;
  let odd = 0;

  for (let i = 0; i < num.length; i++) {
    if (i % 2 === 0) {
      even += Number(num[i]);
    } else {
      odd += Number(num[i]);
    }
  }

  return even === odd;
};
```

---

## Complexity

### Time Complexity

```text
O(n)
```

### Space Complexity

```text
O(1)
```

---

# Approach 3 — Optimal

## Idea

Use only one variable:

- Add even index digits
- Subtract odd index digits

If final result becomes `0`,
both sums are equal.

---

## Code

```js
var isBalanced = function (num) {
  let balance = 0;

  for (let i = 0; i < num.length; i++) {
    if (i % 2 === 0) {
      balance += Number(num[i]);
    } else {
      balance -= Number(num[i]);
    }
  }

  return balance === 0;
};
```

---

# Dry Run

## Input

```js
num = "1230";
```

---

## Step 1

```js
i = 0
balance = 0 + 1 = 1
```

---

## Step 2

```js
i = 1
balance = 1 - 2 = -1
```

---

## Step 3

```js
i = 2
balance = -1 + 3 = 2
```

---

## Step 4

```js
i = 3
balance = 2 - 0 = 2
```

---

## Final

```js
balance !== 0;
```

Return:

```js
false;
```

---

# Complexity Comparison

| Approach    | Time | Space |
| ----------- | ---- | ----- |
| Brute Force | O(n) | O(n)  |
| Better      | O(n) | O(1)  |
| Optimal     | O(n) | O(1)  |

---

# Key Concepts

## Even Index

```js
i % 2 === 0;
```

## Odd Index

```js
i % 2 !== 0;
```

---

# Important Takeaways

## Brute Force

- Uses extra arrays
- More space consumption

---

## Better

- Directly calculates sums
- No extra arrays needed

---

## Optimal

- Uses single variable technique
- Most memory efficient approach

---

# Interview Tip

Whenever a problem asks to:

```text
Compare two groups
```

Think:

```text
Can I use:
+ for one group
- for another group
```

This often leads to an optimal solution.
