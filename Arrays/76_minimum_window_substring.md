# 📘 Minimum Window Substring (LeetCode 76)

## Problem Statement

Given two strings `s` and `t`, return the **minimum window substring** of `s` such that every character in `t` (including duplicates) is included in the window.

If no such substring exists, return:

```js
"";
```

---

## Example

```js
Input: s = "ADOBECODEBANC";
t = "ABC";

Output: "BANC";
```

### Explanation

```text
"BANC" contains:

A → 1 time
B → 1 time
C → 1 time

Smallest valid window
```

---

# Approach 1: Brute Force

## Idea

Generate every possible substring and check whether it contains all characters from `t`.

---

## Algorithm

```text
For every starting index i
    For every ending index j
        Create substring s[i...j]

        Check if substring contains all chars of t

        If valid:
            Update answer
```

---

## Solution

```js
var minWindow = function (s, t) {
  let result = "";

  for (let i = 0; i < s.length; i++) {
    for (let j = i; j < s.length; j++) {
      let sub = s.slice(i, j + 1);

      if (isValid(sub, t)) {
        if (result === "" || sub.length < result.length) {
          result = sub;
        }
      }
    }
  }

  return result;
};

function isValid(sub, t) {
  let need = {};

  for (let ch of t) {
    need[ch] = (need[ch] || 0) + 1;
  }

  for (let ch of sub) {
    if (need[ch]) {
      need[ch]--;
    }
  }

  for (let key in need) {
    if (need[key] > 0) {
      return false;
    }
  }

  return true;
}
```

---

## Complexity

```text
Time  : O(n³)
Space : O(k)
```

Where:

```text
n = length of s
k = unique characters in t
```

---

# Approach 2: Better

## Idea

For each starting index:

```text
Expand right pointer

As soon as window becomes valid:
    Store answer
    Break
```

### Why Break?

For the same starting index:

```text
ABC   ✔ First valid
ABCD
ABCDE
ABCDEF
```

All future windows are larger.

No need to continue.

---

## Solution

```js
var minWindow = function (s, t) {
  let answer = "";

  let need = {};

  for (let ch of t) {
    need[ch] = (need[ch] || 0) + 1;
  }

  for (let i = 0; i < s.length; i++) {
    let window = {};

    for (let j = i; j < s.length; j++) {
      window[s[j]] = (window[s[j]] || 0) + 1;

      if (valid(window, need)) {
        let sub = s.slice(i, j + 1);

        if (answer === "" || sub.length < answer.length) {
          answer = sub;
        }

        break;
      }
    }
  }

  return answer;
};

function valid(window, need) {
  for (let key in need) {
    if ((window[key] || 0) < need[key]) {
      return false;
    }
  }

  return true;
}
```

---

## Complexity

```text
Time  : O(n² × k)
Space : O(k)
```

---

# Approach 3: Optimal (Sliding Window)

## Core Idea

Maintain a window:

```text
[left .... right]
```

### Expand Window

```js
right++;
```

Add characters into the window.

---

### Shrink Window

```js
left++;
```

Remove characters when the window is already valid.

---

# Important Variables

## Need Map

Stores required frequencies.

```js
t = "AABC";

need = {
  A: 2,
  B: 1,
  C: 1,
};
```

---

## Window Map

Stores current frequencies inside the window.

```js
window = {
  A: 2,
  B: 1,
};
```

---

## required

Number of unique characters needed.

```js
need = {
  A: 2,
  B: 1,
  C: 1,
};

required = 3;
```

---

## formed

Number of character requirements currently satisfied.

Example:

```js
window = {
  A: 2,
  B: 1,
};
```

Need:

```js
{
  A:2,
  B:1,
  C:1
}
```

Satisfied:

```text
A ✔
B ✔
C ✖
```

Therefore:

```js
formed = 2;
```

---

# Key Condition #1

```js
if (need.has(char) && window.get(char) === need.get(char)) {
  formed++;
}
```

## Meaning

```text
Character requirement
has just become satisfied.
```

---

### Example

Need:

```js
A: 2;
```

Window progression:

```text
A:1  ❌

A:2  ✅ formed++

A:3  Already satisfied
```

---

## Why Use `===` ?

Because we only count satisfaction once.

```js
2 === 2;
```

Requirement satisfied.

```js
3 === 2;
```

Already satisfied earlier.

No increment.

---

# Valid Window Condition

```js
formed === required;
```

Meaning:

```text
All required characters are present.
```

Example:

```js
formed = 3;
required = 3;
```

Window becomes valid.

---

# Key Condition #2

```js
if (need.has(leftChar) && window.get(leftChar) < need.get(leftChar)) {
  formed--;
}
```

## Meaning

```text
While shrinking,
we removed an important character.
```

Requirement is now broken.

---

### Example

Need:

```js
A: 1;
```

Window:

```js
A: 1;
```

Remove A:

```js
A: 0;
```

Check:

```js
0 < 1;
```

True.

Requirement broken.

```js
formed--;
```

---

# Optimal Solution

```js
var minWindow = function (s, t) {
  if (!s || !t) return "";

  let need = new Map();

  for (let ch of t) {
    need.set(ch, (need.get(ch) || 0) + 1);
  }

  let required = need.size;

  let window = new Map();

  let left = 0;
  let formed = 0;

  let minLen = Infinity;
  let start = 0;

  for (let right = 0; right < s.length; right++) {
    let char = s[right];

    window.set(char, (window.get(char) || 0) + 1);

    if (need.has(char) && window.get(char) === need.get(char)) {
      formed++;
    }

    while (formed === required) {
      if (right - left + 1 < minLen) {
        minLen = right - left + 1;
        start = left;
      }

      let leftChar = s[left];

      window.set(leftChar, window.get(leftChar) - 1);

      if (need.has(leftChar) && window.get(leftChar) < need.get(leftChar)) {
        formed--;
      }

      left++;
    }
  }

  return minLen === Infinity ? "" : s.substring(start, start + minLen);
};
```

---

# Small Dry Run

```js
s = "ABC";
t = "AC";
```

Need:

```js
{
  A:1,
  C:1
}
```

```js
required = 2;
formed = 0;
```

---

## right = 0

```text
A
```

Window:

```js
{
  A: 1;
}
```

Satisfied:

```text
A ✔
```

```js
formed = 1;
```

---

## right = 1

```text
AB
```

Window:

```js
{
 A:1,
 B:1
}
```

```js
formed = 1;
```

---

## right = 2

```text
ABC
```

Window:

```js
{
 A:1,
 B:1,
 C:1
}
```

Satisfied:

```text
A ✔
C ✔
```

```js
formed = 2;
```

Window valid:

```js
formed === required;
```

```text
2 === 2
```

Store answer:

```text
ABC
```

---

### Shrink

Remove A:

```text
BC
```

Now:

```text
A missing
```

```js
formed = 1;
```

Window invalid.

Stop shrinking.

---

## Final Answer

```js
"ABC";
```

---

# Complexity Comparison

| Approach               | Time Complexity | Space Complexity |
| ---------------------- | --------------- | ---------------- |
| Brute Force            | O(n³)           | O(k)             |
| Better                 | O(n² × k)       | O(k)             |
| Optimal Sliding Window | O(n + m)        | O(k)             |

Where:

```text
n = length of s
m = length of t
k = unique characters in t
```

---

# Sliding Window Pattern

```js
for (let right = 0; right < n; right++) {

    // Expand window

    while (window is valid) {

        // Update answer

        // Shrink window
    }
}
```

---

# Interview Takeaway

```text
1. Build need frequency map.
2. Expand right pointer.
3. Track satisfied requirements using formed.
4. When formed == required, window is valid.
5. Update answer.
6. Shrink from left.
7. Repeat until end.
```

## Memory Trick

```text
Expand right until valid.

Shrink left until invalid.

Repeat.
```

This is one of the most important **Variable Size Sliding Window** interview patterns and is the foundation for problems like:

- Longest Repeating Character Replacement
- Permutation in String
- Find All Anagrams in a String
- Fruit Into Baskets
- Smallest Subarray with Given Sum
- Minimum Window Substring 🚀
