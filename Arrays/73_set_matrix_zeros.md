# 73. Set Matrix Zeroes

## Problem Statement

Given an `m x n` matrix, if an element is `0`, set its entire row and column to `0`.

You must do it **in-place**.

---

# Example

## Input

```js
[
  [1, 1, 1],
  [1, 0, 1],
  [1, 1, 1],
];
```

## Output

```js
[
  [1, 0, 1],
  [0, 0, 0],
  [1, 0, 1],
];
```

---

# Approach 1 — Brute Force

## Idea

Whenever a `0` is found:

- mark entire row
- mark entire column

But directly changing to `0` creates cascading updates.

So use a temporary marker like `-1`.

Finally convert all `-1 → 0`.

---

## Code

```javascript
var setZeroes = function (matrix) {
  let rows = matrix.length;
  let cols = matrix[0].length;

  function markRow(i) {
    for (let j = 0; j < cols; j++) {
      if (matrix[i][j] !== 0) {
        matrix[i][j] = -1;
      }
    }
  }

  function markCol(j) {
    for (let i = 0; i < rows; i++) {
      if (matrix[i][j] !== 0) {
        matrix[i][j] = -1;
      }
    }
  }

  // Mark rows & cols
  for (let i = 0; i < rows; i++) {
    for (let j = 0; j < cols; j++) {
      if (matrix[i][j] === 0) {
        markRow(i);
        markCol(j);
      }
    }
  }

  // Convert -1 → 0
  for (let i = 0; i < rows; i++) {
    for (let j = 0; j < cols; j++) {
      if (matrix[i][j] === -1) {
        matrix[i][j] = 0;
      }
    }
  }
};
```

---

## Complexity

| Complexity | Value                |
| ---------- | -------------------- |
| Time       | O((m × n) × (m + n)) |
| Space      | O(1)                 |

---

## Trade-Offs

### Pros

- Simple logic
- Easy to understand

### Cons

- Slow for large matrices
- Unsafe if matrix already contains `-1`

---

# Approach 2 — Better Solution

## Idea

Use two extra arrays:

```js
row[]
col[]
```

Store which rows and columns must become zero.

---

## Code

```javascript
var setZeroes = function (matrix) {
  let rows = matrix.length;
  let cols = matrix[0].length;

  let row = new Array(rows).fill(0);
  let col = new Array(cols).fill(0);

  // Store markers
  for (let i = 0; i < rows; i++) {
    for (let j = 0; j < cols; j++) {
      if (matrix[i][j] === 0) {
        row[i] = 1;
        col[j] = 1;
      }
    }
  }

  // Fill zeros
  for (let i = 0; i < rows; i++) {
    for (let j = 0; j < cols; j++) {
      if (row[i] === 1 || col[j] === 1) {
        matrix[i][j] = 0;
      }
    }
  }
};
```

---

## Complexity

| Complexity | Value    |
| ---------- | -------- |
| Time       | O(m × n) |
| Space      | O(m + n) |

---

## Trade-Offs

### Pros

- Clean logic
- Easier debugging
- Faster than brute force

### Cons

- Uses extra memory

---

# Approach 3 — Optimal Solution

## Core Idea

Reuse matrix itself for storing markers.

Use:

```js
matrix[i][0];
```

as row marker.

Use:

```js
matrix[0][j];
```

as column marker.

---

# Important Conflict

```js
matrix[0][0];
```

belongs to BOTH:

- first row
- first column

So we separately track:

```js
firstRowZero;
firstColZero;
```

---

# Algorithm

## Step 1 — Check first row

If first row contains zero:

```js
firstRowZero = true;
```

---

## Step 2 — Check first column

If first column contains zero:

```js
firstColZero = true;
```

---

## Step 3 — Mark rows & columns

Whenever zero found:

```js
matrix[i][0] = 0;
matrix[0][j] = 0;
```

---

## Step 4 — Fill zeros using markers

If:

```js
matrix[i][0] === 0;
OR;
matrix[0][j] === 0;
```

make current cell zero.

---

## Step 5 — Handle first row

If:

```js
firstRowZero === true;
```

make first row zero.

---

## Step 6 — Handle first column

If:

```js
firstColZero === true;
```

make first column zero.

---

# Optimal Code

```javascript
var setZeroes = function (matrix) {
  let rows = matrix.length;
  let cols = matrix[0].length;

  let firstRowZero = false;
  let firstColZero = false;

  // Check first row
  for (let j = 0; j < cols; j++) {
    if (matrix[0][j] === 0) {
      firstRowZero = true;
      break;
    }
  }

  // Check first column
  for (let i = 0; i < rows; i++) {
    if (matrix[i][0] === 0) {
      firstColZero = true;
      break;
    }
  }

  // Mark rows & columns
  for (let i = 1; i < rows; i++) {
    for (let j = 1; j < cols; j++) {
      if (matrix[i][j] === 0) {
        matrix[i][0] = 0;
        matrix[0][j] = 0;
      }
    }
  }

  // Fill zeros
  for (let i = 1; i < rows; i++) {
    for (let j = 1; j < cols; j++) {
      if (matrix[i][0] === 0 || matrix[0][j] === 0) {
        matrix[i][j] = 0;
      }
    }
  }

  // Handle first row
  if (firstRowZero) {
    for (let j = 0; j < cols; j++) {
      matrix[0][j] = 0;
    }
  }

  // Handle first column
  if (firstColZero) {
    for (let i = 0; i < rows; i++) {
      matrix[i][0] = 0;
    }
  }
};
```

---

# Dry Run

## Input

```js
[
  [1, 1, 1],
  [1, 0, 1],
  [1, 1, 1],
];
```

---

## After Marking

```js
[
  [1, 0, 1],
  [0, 0, 1],
  [1, 1, 1],
];
```

Meaning:

- Row 1 must become zero
- Column 1 must become zero

---

## Final Output

```js
[
  [1, 0, 1],
  [0, 0, 0],
  [1, 0, 1],
];
```

---

# Why loops start from 1?

Because first row and first column are used as markers.

If loops start from `0`,
markers get overwritten.

---

# Complexity Comparison

| Approach    | Time          | Space  |
| ----------- | ------------- | ------ |
| Brute Force | O((m×n)(m+n)) | O(1)   |
| Better      | O(m×n)        | O(m+n) |
| Optimal     | O(m×n)        | O(1)   |

---

# Interview Discussion

## ▶️ Why did you do it this way?

The optimal approach minimizes extra space.

Instead of using additional arrays:

```js
row[]
col[]
```

the matrix itself stores metadata.

This reduces:

```js
O(m + n);
```

extra space to:

```js
O(1);
```

---

## ▶️ What would you change if traffic grows 10x?

For extremely large matrices:

### Improvements

#### 1. Sparse Matrix Optimization

If zeros are rare:

Store only zero positions using:

```js
Set;
Map;
```

instead of scanning repeatedly.

---

#### 2. Parallel Processing

Separate chunks of matrix can be processed concurrently.

---

#### 3. Distributed Systems

Huge matrices can be partitioned across nodes.

---

#### 4. Cache Optimization

Current row-wise traversal is already cache friendly.

---

## ▶️ Can you debug this live?

Yes.

### Common Bugs

#### Bug 1 — Forgetting first row/column conflict

```js
matrix[0][0];
```

cannot represent both row and column.

Fix:

```js
firstRowZero;
firstColZero;
```

---

#### Bug 2 — Starting loops from 0

Overwrites markers.

Correct:

```js
for (let i = 1; ...)
```

---

#### Bug 3 — Updating matrix immediately

Creates cascading zeros.

Fix:

Use two phases:

1. Marking
2. Modification

---

### Debug Strategy

Print matrix after marking phase:

```js
console.log(matrix);
```

Verify:

- row markers
- column markers

before applying zeros.

---

## ▶️ What trade-offs did you make, and why?

### Brute Force

#### Trade-Off

- simple implementation
- poor performance

---

### Better Solution

#### Trade-Off

- extra memory
- easier debugging

---

### Optimal Solution

#### Trade-Off

- more complex logic
- harder debugging
- minimal extra space

Chosen because interview expects:

```js
O(1);
```

space optimization.

---

## ▶️ How did you test this?

---

### Test 1 — Normal Case

```js
[
  [1, 1, 1],
  [1, 0, 1],
  [1, 1, 1],
];
```

---

### Test 2 — Multiple Zeros

```js
[
  [0, 1],
  [1, 0],
];
```

---

### Test 3 — Zero in First Row

```js
[[1, 0, 1]];
```

Checks:

```js
firstRowZero;
```

---

### Test 4 — Zero in First Column

```js
[[1], [0], [1]];
```

Checks:

```js
firstColZero;
```

---

### Test 5 — Entire Matrix Zero

```js
[
  [0, 0],
  [0, 0],
];
```

---

### Test 6 — No Zero Present

```js
[
  [1, 2],
  [3, 4],
];
```

Matrix should remain unchanged.

---

### Test 7 — Single Cell

```js
[[0]];
```

and

```js
[[1]];
```

---

# Pattern Learned

## Matrix as Storage Optimization

Reuse existing matrix for metadata instead of extra arrays.

Very important DSA interview pattern.
