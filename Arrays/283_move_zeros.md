# 🚀 Move Zeros (DSA)

## 🧩 Problem

Given an array `nums`, move all `0`s to the end while maintaining the **relative order of non-zero elements**.

### ✅ Constraints

- Must be **in-place** (preferred)
- Preserve order of non-zero elements

### 📌 Example

```js
Input: [0, 1, 0, 3, 12];
Output: [1, 3, 12, 0, 0];
```

---

# 🧠 Approach 1: Brute Force

## 💡 Idea

- Collect all **non-zero elements**
- Append zeros at the end

## ❌ Drawback

- Uses extra space → not in-place

## 💻 Code

```js
const moveZerosBrute = (nums) => {
  let temp = [];

  for (let num of nums) {
    if (num !== 0) temp.push(num);
  }

  while (temp.length < nums.length) {
    temp.push(0);
  }

  return temp;
};
```

## ⏱ Complexity

- Time: `O(n)`
- Space: `O(n)` ❌

---

# ⚡ Approach 2: Better (Overwrite Method)

## 💡 Idea

- Move all non-zero elements to the front
- Fill remaining positions with zero

## 💻 Code

```js
const moveZerosBetter = (nums) => {
  let left = 0;

  for (let i = 0; i < nums.length; i++) {
    if (nums[i] !== 0) {
      nums[left++] = nums[i];
    }
  }

  while (left < nums.length) {
    nums[left++] = 0;
  }

  return nums;
};
```

## ⏱ Complexity

- Time: `O(n)`
- Space: `O(1)` ✅

## ⚠️ Notes

- Requires **two passes**
- Overwrites values

---

# 🏆 Approach 3: Optimal (Two Pointer Swap)

## 💡 Idea

- Use two pointers:

  - `i` → traverses array
  - `j` → position for next non-zero

- Swap non-zero elements forward

## 💻 Code

```js
const moveZerosOptimal = (nums) => {
  let j = 0;

  for (let i = 0; i < nums.length; i++) {
    if (nums[i] !== 0) {
      [nums[i], nums[j]] = [nums[j], nums[i]];
      j++;
    }
  }

  return nums;
};
```

---

# 🔍 Pointer Explanation

| Pointer | Role                              |
| ------- | --------------------------------- |
| `i`     | Traverses array                   |
| `j`     | Tracks position for next non-zero |

---

# ⚡ Why It Works

- Non-zero elements are moved forward
- Zeros automatically shift to the end
- Order is preserved (**stable approach**)

---

# ⏱ Complexity

- Time: `O(n)`
- Space: `O(1)` ✅

---

# 🧠 Edge Cases

```js
[0,0,0] → [0,0,0]
[1,2,3] → [1,2,3]
[] → []
[0,1] → [1,0]
```

---

# ⚖️ Trade-offs

| Approach | Space | Passes | Best Use            |
| -------- | ----- | ------ | ------------------- |
| Brute    | O(n)  | 2      | Easy explanation    |
| Better   | O(1)  | 2      | Safe approach       |
| Optimal  | O(1)  | 1      | Best for interviews |

---

# 🎯 Interview Answer

> I would use a **two-pointer approach**.
> One pointer tracks where the next non-zero element should go.
> While iterating, I swap non-zero elements forward, maintaining order and pushing zeros to the end.
> This gives **O(n) time and O(1) space**, which is optimal.

---

# ✅ Test

```js
console.info(moveZerosBrute([0, 1, 0, 3, 12]));
console.info(moveZerosBetter([0, 1, 0, 3, 12]));
console.info(moveZerosOptimal([0, 1, 0, 3, 12]));
```

---

# 🧩 Key Takeaway

👉 This is a classic **Two Pointer + In-place modification** problem
👉 Very common in interviews (Google, Amazon, etc.)
👉 Focus on **stability + space optimization**
