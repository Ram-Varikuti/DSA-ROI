---

# 🧠 Missing Number (DSA)

![JavaScript](https://img.shields.io/badge/Language-JavaScript-yellow)
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-green)
![Time Complexity](<https://img.shields.io/badge/Time-O(n)-blue>)
![Space Complexity](<https://img.shields.io/badge/Space-O(1)-orange>)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)

---

## 📌 Problem

Given an array of size `n` containing numbers from `0 → n`, one number is missing.

👉 Find the missing number.

### Example

```js
Input: [3, 0, 1];
Output: 2;
```

---

## 🚀 Approaches

---

### 🐢 1. Brute Force

**Idea:**
Check every number from `0 → n`

```js
const missingNumberBrute = (nums) => {
  for (let i = 0; i <= nums.length; i++) {
    if (!nums.includes(i)) return i;
  }
};
```

**Complexity**

- Time: `O(n²)`
- Space: `O(1)`

❌ Not suitable for large inputs

---

### ⚡ 2. Better (Using Set)

**Idea:**
Store elements in a Set

```js
const missingNumberBetter = (nums) => {
  const set = new Set(nums);

  for (let i = 0; i <= nums.length; i++) {
    if (!set.has(i)) return i;
  }
};
```

**Complexity**

- Time: `O(n)`
- Space: `O(n)`

⚠️ Uses extra memory

---

### ⭐ 3. Optimal (Sum Formula)

**Idea:**
Expected sum − Actual sum

```js
const missingNumberOptimal = (nums) => {
  const n = nums.length;

  const expectedSum = (n * (n + 1)) / 2;
  const actualSum = nums.reduce((a, b) => a + b, 0);

  return expectedSum - actualSum;
};
```

**Complexity**

- Time: `O(n)`
- Space: `O(1)`

✅ Best for interviews

---

### 💥 4. Optimal (XOR Approach)

**Idea:**
Cancel common elements using XOR

```js
const missingNumberXOR = (nums) => {
  let xor = nums.length;

  for (let i = 0; i < nums.length; i++) {
    xor ^= i ^ nums[i];
  }

  return xor;
};
```

**Complexity**

- Time: `O(n)`
- Space: `O(1)`

⭐ Advanced approach

---

## 🔑 XOR Properties

```
a ^ a = 0
a ^ 0 = a
a ^ b = b ^ a
(a ^ b) ^ c = a ^ (b ^ c)
```

👉 Same numbers cancel out

---

## 🧪 Edge Cases

```
[0] → 1
[1] → 0
[0,1,2,3] → 4
```

---

## ⚠️ Constraints

- Numbers must be from `0 → n`
- Exactly one missing
- No duplicates

---

## 📈 Scalability

| Approach | Time  | Space | Suitable |
| -------- | ----- | ----- | -------- |
| Brute    | O(n²) | O(1)  | ❌       |
| Set      | O(n)  | O(n)  | ⚠️       |
| Sum      | O(n)  | O(1)  | ✅       |
| XOR      | O(n)  | O(1)  | ⭐       |

---

## 🐞 Debug Checklist

- Check `n = nums.length`
- Validate range `0 → n`
- Avoid off-by-one errors
- Ensure only one missing number

---

## 🎯 Interview Tip

If asked:

> "Solve without extra space"

👉 Use:

- Sum Formula ✅
- XOR ⭐

---

## 🔥 Pattern Recognition

Use XOR when:

- One number missing
- One number repeating
- Finding unique element
- Pair cancellation problems

---
