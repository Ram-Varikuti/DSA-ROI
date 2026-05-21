# 🍎 904. Fruit Into Baskets

## 📌 Problem Statement

You are given an integer array `fruits` where:

```txt id="jsllwm"
fruits[i] = type of fruit on the ith tree
```

You have:

- 🧺 2 baskets
- Each basket can store only **one fruit type**
- You must pick fruits from a **continuous subarray**

Return:

```txt id="ktllwm"
Maximum number of fruits you can collect
```

---

# 🧠 Key Observation

We need:

```txt id="lullwm"
Longest subarray containing at most 2 distinct elements
```

This is a classic:

# 🚀 Sliding Window + HashMap Problem

---

# 📌 Example 1

```js id="mvllwm"
Input: [1, 2, 1];

Output: 3;
```

### Explanation

Entire array contains only:

```txt id="nwllwm"
1 and 2
```

So we can collect all fruits.

---

# 📌 Example 2

```js id="oxllwm"
Input: [0, 1, 2, 2];

Output: 3;
```

### Explanation

Best valid window:

```txt id="pyllwm"
[1,2,2]
```

Contains only:

```txt id="qzllwm"
1 and 2
```

Length:

```txt id="r0llwm"
3
```

---

# 📌 Example 3

```js id="s1llwm"
Input: [1, 2, 3, 2, 2];

Output: 4;
```

### Explanation

Best valid window:

```txt id="t2llwm"
[2,3,2,2]
```

Contains:

```txt id="u3llwm"
2 and 3
```

Length:

```txt id="v4llwm"
4
```

---

# 💡 Intuition

We maintain:

- a sliding window
- only 2 fruit types inside the window

If window becomes invalid:

```txt id="w5llwm"
(map.size > 2)
```

shrink from left.

Track maximum valid window size.

---

# ✅ Optimal Solution

```js id="x6llwm"
var totalFruit = function (fruits) {
  let left = 0;
  let maxLen = 0;

  // fruit -> count
  let map = new Map();

  for (let right = 0; right < fruits.length; right++) {
    // Add current fruit
    map.set(fruits[right], (map.get(fruits[right]) || 0) + 1);

    // If more than 2 fruit types
    while (map.size > 2) {
      // Reduce left fruit count
      map.set(fruits[left], map.get(fruits[left]) - 1);

      // Remove fruit if count becomes 0
      if (map.get(fruits[left]) === 0) {
        map.delete(fruits[left]);
      }

      // Shrink window
      left++;
    }

    // Update maximum valid window size
    maxLen = Math.max(maxLen, right - left + 1);
  }

  return maxLen;
};
```

---

# 🔍 Dry Run

## Input

```js id="y7llwm"
[1, 2, 3, 2, 2];
```

---

## Step 1

```txt id="z8llwm"
window = [1]

map = {1:1}

maxLen = 1
```

---

## Step 2

```txt id="09mlwm"
window = [1,2]

map = {1:1,2:1}

maxLen = 2
```

---

## Step 3

```txt id="1amlwm"
window = [1,2,3]

map = {1:1,2:1,3:1}
```

❌ Invalid (3 fruit types)

Shrink window:

Remove `1`

```txt id="2bmlwm"
window = [2,3]

map = {2:1,3:1}
```

---

## Step 4

```txt id="3cmlwm"
window = [2,3,2]

map = {2:2,3:1}

maxLen = 3
```

---

## Step 5

```txt id="4dmlwm"
window = [2,3,2,2]

map = {2:3,3:1}

maxLen = 4
```

✅ Final Answer:

```txt id="5emlwm"
4
```

---

# 🧩 Important Concepts

---

## 1️⃣ Why Sliding Window?

Because:

- subarray must be continuous
- dynamically maintain:

  ```txt id="6fmlwm"
  at most 2 distinct elements
  ```

---

## 2️⃣ Why HashMap?

Stores:

```txt id="7gmlwm"
fruit -> count
```

Example:

```js id="8hmlwm"
{
  2 => 3,
  3 => 1
}
```

---

## 3️⃣ Why `while (map.size > 2)` ?

If more than 2 fruit types exist:

```txt id="9imlwm"
window becomes invalid
```

So shrink from left until valid again.

---

## 4️⃣ Why Delete Fruit?

```js id="ajmlwm"
if (map.get(fruits[left]) === 0) {
  map.delete(fruits[left]);
}
```

If count becomes `0`,
fruit no longer exists in window.

---

## 5️⃣ Window Length Formula

```js id="bkmlwm"
right - left + 1;
```

Example:

```txt id="clmlwm"
left = 1
right = 4

Length = 4 - 1 + 1 = 4
```

---

## 6️⃣ Why `Math.max`?

```js id="dmlmwm"
maxLen = Math.max(maxLen, right - left + 1);
```

Tracks:

```txt id="enmlwm"
largest valid window found so far
```

---

# ⏱ Complexity Analysis

## ✅ Time Complexity

```txt id="fomlwm"
O(n)
```

Each element:

- added once
- removed once

---

## ✅ Space Complexity

```txt id="gpmlwm"
O(1)
```

Map stores at most 3 fruit types.

---

# 📚 Pattern Recognition

Whenever you see:

- longest subarray
- at most K distinct elements
- continuous sequence

Think:

# 🔥 Sliding Window + HashMap

---

# 🧠 Generic Template

```js id="hqmlwm"
for (right pointer) {

    add current element

    while (window invalid) {
        remove from left
    }

    update answer
}
```

---

# ⚖️ Trade-offs

## ❌ Brute Force

Check all subarrays.

```txt id="irmlwm"
Time: O(n²)
```

Not scalable.

---

## ✅ Sliding Window

Efficient dynamic window.

```txt id="jsmlwm"
Time: O(n)
```

Best approach.

---

# 🛠 Debugging Strategy

If answer is wrong:

## Print Window

```js id="ktmlwm"
console.log(left, right);
```

---

## Print Map

```js id="lumlwm"
console.log(map);
```

---

## Verify

```txt id="mvmlwm"
map.size should never exceed 2
after shrinking
```

---

# 🧪 Edge Cases

## Single Element

```js id="nwmlwm"
[1];
```

Output:

```txt id="oxmlwm"
1
```

---

## All Same Fruits

```js id="pymlwm"
[2, 2, 2];
```

Output:

```txt id="qzmlwm"
3
```

---

## More Than 2 Types

```js id="r0mlwm"
[1, 2, 3, 4];
```

Output:

```txt id="s1mlwm"
2
```

---

# 🚀 Scalability

Even if input grows 10x:

- still linear traversal
- no nested loops
- memory efficient

Works efficiently for large-scale input sizes.

---

# 🎯 Final Takeaway

This problem is fundamentally:

```txt id="t2mlwm"
Longest Subarray with At Most 2 Distinct Elements
```

Master this pattern because it appears in:

- Longest substring problems
- Character replacement
- Binary subarray problems
- Frequency-based sliding window questions

Very important interview pattern 🚀
