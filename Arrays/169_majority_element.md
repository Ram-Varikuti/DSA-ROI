# ⭐ Majority Element (LeetCode 169)

## 🧾 Problem

Given an array `nums` of size `n`, find the element that appears **more than ⌊n/2⌋ times**.

👉 Majority element is guaranteed in base problem.

---

# 🥉 1. Brute Force

## 💡 Idea

- For each element → count its frequency
- Return if count > n/2

## ✅ Code

```js
const majorityElementBrute = (nums) => {
  for (let i = 0; i < nums.length; i++) {
    let count = 0;

    for (let j = 0; j < nums.length; j++) {
      if (nums[i] === nums[j]) count++;
    }

    if (count > nums.length / 2) return nums[i];
  }
};
```

## ⏱️ Complexity

- Time: O(n²)
- Space: O(1)

---

# 🥈 2. Better (Hash Map)

## 💡 Idea

- Store frequency in map
- Return when count > n/2

## ✅ Code

```js
const majorityElementBetter = (nums) => {
  let map = {};

  for (let num of nums) {
    map[num] = (map[num] || 0) + 1;

    if (map[num] > nums.length / 2) {
      return num;
    }
  }
};
```

## ⏱️ Complexity

- Time: O(n)
- Space: O(n)

---

# 🥇 3. Optimal (Boyer–Moore Voting)

## 💡 Idea

👉 Cancel different elements → majority survives

## ✅ Code

```js
const majorityElementOptimal = (nums) => {
  let count = 0;
  let candidate = null;

  for (let num of nums) {
    if (count === 0) candidate = num;
    count += num === candidate ? 1 : -1;
  }

  return candidate;
};
```

## ⏱️ Complexity

- Time: O(n)
- Space: O(1)

---

# 🔐 Safe Version (When Majority NOT Guaranteed)

## ✅ Code

```js
var majorityElement = function (nums) {
  let count = 0;
  let candidate = null;

  // Phase 1: Find candidate
  for (let num of nums) {
    if (count === 0) candidate = num;
    count += num === candidate ? 1 : -1;
  }

  // Phase 2: Verify
  count = 0;
  for (let num of nums) {
    if (num === candidate) count++;
  }

  return count > nums.length / 2 ? candidate : -1;
};
```

---

# 🧠 Key Intuition

👉 Same element → +1
👉 Different element → -1 (cancel)
👉 Count = 0 → reset candidate

✔ Majority element **cannot be fully cancelled**

---

# ⚡ Mental Trick

👉 “Increase for same, decrease for different, reset at zero”

OR

👉 “Cancel pairs → last survivor wins”

---

# 🧪 Example

```
[3,2,3]
→ 3 survives → answer = 3
```

---

# ⚠️ Important Points

- Boyer–Moore works **only if majority exists**
- If NOT guaranteed → always do **second pass**

---

# ⚖️ Trade-offs

| Approach | Time  | Space | Notes |
| -------- | ----- | ----- | ----- |
| Brute    | O(n²) | O(1)  | Slow  |
| HashMap  | O(n)  | O(n)  | Easy  |
| Optimal  | O(n)  | O(1)  | Best  |

---

# 🔍 Debug Tip

```js
console.log(num, candidate, count);
```

---

# 🧪 Edge Cases

- Single element → return it
- All same elements
- No majority (safe version → -1)
- Negative numbers

---

# 🚀 Interview Questions

### ▶️ Why Boyer–Moore works?

Because majority count > all other elements combined

### ▶️ What if no majority?

Add verification step

### ▶️ Why better than HashMap?

No extra memory

---

# 🧠 One-line Summary

👉 “Cancel different elements — majority survives.”

---

# 🔥 Next Step

👉 Majority Element II (n/3) — common follow-up
