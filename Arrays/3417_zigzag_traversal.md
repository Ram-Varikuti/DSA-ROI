# Zigzag Grid Traversal With Skipping

## Problem Statement

Given an `m x n` 2D grid of positive integers:

- Traverse the grid in **zigzag order**
- Skip every alternate cell
- Return the visited elements

---

# Zigzag Traversal Pattern

Traversal direction changes for every row.

## Even Rows → Left to Right

```txt
Row 0  → → →
Row 2  → → →
Row 4  → → →
```

Condition:

```js
if (i % 2 === 0)
```

---

## Odd Rows → Right to Left

```txt
← ← ←  Row 1
← ← ←  Row 3
← ← ←  Row 5
```

Handled using:

```js
else
```

---

# Understanding `%` Operator

`%` gives remainder.

```js
0 % 2 = 0
1 % 2 = 1
2 % 2 = 0
3 % 2 = 1
```

### Even Number Check

```js
i % 2 === 0;
```

Meaning:

- TRUE → even row
- FALSE → odd row

---

# Skip Alternate Cells

We use a boolean variable:

```js
let skip = false;
```

Meaning:

| skip  | Action       |
| ----- | ------------ |
| false | Take element |
| true  | Skip element |

---

# Toggle Skip

```js
skip = !skip;
```

This flips the value:

```txt
false → true
true → false
```

Pattern becomes:

```txt
Take → Skip → Take → Skip
```

---

# Approach

1. Traverse row by row
2. If row is even:

   - move left → right

3. If row is odd:

   - move right → left

4. Add element only when `skip == false`
5. Toggle skip after every cell

---

# JavaScript Solution

```js
var zigzagTraversal = function (grid) {
  let result = [];
  let skip = false;

  for (let i = 0; i < grid.length; i++) {
    // Even row → Left to Right
    if (i % 2 === 0) {
      for (let j = 0; j < grid[0].length; j++) {
        if (!skip) {
          result.push(grid[i][j]);
        }

        skip = !skip;
      }
    }

    // Odd row → Right to Left
    else {
      for (let j = grid[0].length - 1; j >= 0; j--) {
        if (!skip) {
          result.push(grid[i][j]);
        }

        skip = !skip;
      }
    }
  }

  return result;
};
```

---

# Dry Run

## Input

```js
grid = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9],
];
```

---

# Step 1 → Zigzag Traversal

```txt
1 → 2 → 3
          ↓
6 ← 5 ← 4
↓
7 → 8 → 9
```

Traversal sequence:

```txt
1, 2, 3, 6, 5, 4, 7, 8, 9
```

---

# Step 2 → Skip Alternate Cells

| Value | Action |
| ----- | ------ |
| 1     | Take   |
| 2     | Skip   |
| 3     | Take   |
| 6     | Skip   |
| 5     | Take   |
| 4     | Skip   |
| 7     | Take   |
| 8     | Skip   |
| 9     | Take   |

---

# Output

```js
[1, 3, 5, 7, 9];
```

---

# Time Complexity

```txt
O(m × n)
```

Every cell is visited once.

---

# Space Complexity

```txt
O(1)
```

Ignoring output array.

---

# Key Takeaways

✅ `% 2` checks even/odd row
✅ Even rows move left → right
✅ Odd rows move right → left
✅ `skip` controls take/skip logic
✅ `skip = !skip` toggles boolean
✅ Zigzag traversal + skipping handled together
