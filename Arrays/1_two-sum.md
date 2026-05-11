# 🔢 Two Sum (LeetCode #1)

## 📝 Problem Statement

Given an array `nums` and an integer `target`, return indices of two numbers such that:

```text
nums[i] + nums[j] = target  (i ≠ j)
```

### ✅ Constraints

- Exactly **one solution exists**
- Cannot use the same element twice
- Return **indices**, not values

---

# 🧠 Approaches Overview

| Approach    | Time Complexity | Space | Works on Unsorted | Key Idea             |
| ----------- | --------------- | ----- | ----------------- | -------------------- |
| Brute Force | O(n²)           | O(1)  | ✅ Yes            | Check all pairs      |
| HashMap     | O(n)            | O(n)  | ✅ Yes            | Store complements    |
| Two Pointer | O(n log n)      | O(n)  | ❌ Needs sorting  | Use sorted direction |

---

# 🥉 1. Brute Force

## 💡 Idea

Check every pair

## ⚙️ Code

```js
const twoSumBrute = (nums, target) => {
  for (let i = 0; i < nums.length; i++) {
    for (let j = i + 1; j < nums.length; j++) {
      if (nums[i] + nums[j] === target) {
        return [i, j];
      }
    }
  }
};
```

## ⏱ Complexity

- Time: **O(n²)**
- Space: **O(1)**

## ⚠️ Limitation

- Not scalable for large inputs

---

# 🥈 2. Better (Sorting + Two Pointer)

## 💡 Idea

- Store `[value, index]`
- Sort array
- Use two-pointer

---

## ⚙️ Code

```js
const twoSumBetter = (nums, target) => {
  let arr = nums.map((val, idx) => [val, idx]);
  arr.sort((a, b) => a[0] - b[0]);

  let l = 0,
    r = arr.length - 1;

  while (l < r) {
    let sum = arr[l][0] + arr[r][0];

    if (sum === target) {
      return [arr[l][1], arr[r][1]];
    } else if (sum < target) {
      l++; // need bigger sum
    } else {
      r--; // need smaller sum
    }
  }
};
```

---

## 🔍 Key Steps

### 1️⃣ Preserve indices

```js
[val, idx];
```

### 2️⃣ Sort values

```js
arr.sort((a, b) => a[0] - b[0]);
```

### 3️⃣ Two-pointer logic

- `sum < target` → `l++`
- `sum > target` → `r--`
- `sum === target` → ✅

---

## 🧠 Key Insight

> Sorted array gives control — left increases sum, right decreases sum

---

## ⏱ Complexity

- Time: **O(n log n)**
- Space: **O(n)**

---

## ⚠️ Important

- ❌ Does NOT work on unsorted arrays directly
- ❌ Must store indices before sorting

---

# 🥇 3. Optimal (HashMap)

## 💡 Idea

Store visited numbers and check complement

---

## ⚙️ Code

```js
const twoSumOptimal = (nums, target) => {
  let map = new Map();

  for (let i = 0; i < nums.length; i++) {
    let needed = target - nums[i];

    if (map.has(needed)) {
      return [map.get(needed), i];
    }

    map.set(nums[i], i);
  }
};
```

---

## 🔍 How it works

```text
target = 9

num = 2 → need 7 → store 2
num = 7 → need 2 → found in map ✅
```

---

## ⏱ Complexity

- Time: **O(n)**
- Space: **O(n)**

---

## 🧠 Key Insight

> Instead of searching, remember what you've seen

---

# 🚀 Scalability Analysis

## 📊 When Input Grows (n → 10⁵, 10⁶)

---

## 🥉 Brute Force

```text
O(n²) → 10^10 operations ❌
```

- Will **timeout / crash**
- Not usable in real systems

---

## 🥈 Two Pointer

```text
O(n log n) ≈ 1.7M operations (for n=10⁵) ✅
```

- Handles large input well
- Slight overhead due to sorting

---

## 🥇 HashMap

```text
O(n) → 10⁵ operations ✅
```

- Best scalability
- Single pass

---

## ⚖️ Real-World Trade-offs

| Scenario                   | Best Choice           |
| -------------------------- | --------------------- |
| Very large data (millions) | HashMap ✅            |
| Memory constrained         | Two Pointer ⚖️        |
| Already sorted data        | Two Pointer (O(n)) ✅ |
| Small input                | Any works             |

---

## 🧠 System-Level Thinking

- If traffic grows **10x**:

  - Brute → ❌ fails
  - Two Pointer → ✅ works
  - HashMap → ✅ best

- If memory is limited:

  - Prefer **Two Pointer**

- If streaming data:

  - Prefer **HashMap**

---

## ⚡ Advanced Insight

- HashMap:

  - Possible hash collisions (rare)
  - Higher memory usage

- Two Pointer:

  - Better cache locality
  - Predictable memory usage

---

## 🎯 Interview One-Liner

> “HashMap is optimal for time O(n), but Two Pointer is useful when sorting is allowed or memory is constrained. Brute force is baseline but not scalable.”

---

# 🧪 Edge Cases

- `[2, 7]` → minimum input
- Negative numbers → works
- Duplicate values → handled
- Large inputs → prefer HashMap

---

# 🐞 Debug Checklist

### 🔹 General

- Returning **indices**, not values?

### 🔹 HashMap

- Check `map.has()` BEFORE inserting?

### 🔹 Two Pointer

- Stored `[value, index]`?
- Sorted correctly?
- Pointer movement correct?

  - `l++` → increase sum
  - `r--` → decrease sum

---

# 🧠 Final Mental Model

```text
Brute Force  → Try everything
HashMap      → Remember past
Two Pointer  → Use order
```
