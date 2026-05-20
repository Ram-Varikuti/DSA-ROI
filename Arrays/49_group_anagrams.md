# 49. Group Anagrams — Complete Notes

Data Structures and Algorithms

# Problem

Given an array of strings `strs`, group the anagrams together.

Two strings are anagrams if:

- same characters
- same frequency

---

# Example

```js id="3vhy0j"
Input: ["eat", "tea", "tan", "ate", "nat", "bat"];
```

Output:

```js id="stz17h"
[["eat", "tea", "ate"], ["tan", "nat"], ["bat"]];
```

---

# Approach 1 → Brute Force

## Idea

Compare every string with every other string.

If they are anagrams:

- put them into same group

To check anagrams:

- sort both strings
- compare

---

# Algorithm

For every string:

- compare with remaining strings
- if sorted versions match:

  - same group

---

# Code

```js id="ay7xqs"
var groupAnagrams = function (strs) {
  let result = [];
  let visited = new Array(strs.length).fill(false);

  for (let i = 0; i < strs.length; i++) {
    if (visited[i]) continue;

    let group = [strs[i]];
    visited[i] = true;

    for (let j = i + 1; j < strs.length; j++) {
      let s1 = strs[i].split("").sort().join("");
      let s2 = strs[j].split("").sort().join("");

      if (s1 === s2) {
        group.push(strs[j]);
        visited[j] = true;
      }
    }

    result.push(group);
  }

  return result;
};
```

---

# Complexity

## Time Complexity

Comparing every pair:

O(n^2 \cdot k \log k)

---

# Space Complexity

O(n)

---

# Drawback

Very slow for large input.

---

# Approach 2 → Sorting + HashMap (Most Common)

# Core Idea

If two strings are anagrams:

```txt id="vl68hh"
eat -> aet
tea -> aet
ate -> aet
```

Sorted strings become identical.

Use sorted string as hashmap key.

---

# Algorithm

For every string:

1. sort characters
2. use sorted string as key
3. store original string in hashmap

---

# Code

```js id="km9f1p"
var groupAnagrams = function (strs) {
  let map = {};

  for (let str of strs) {
    // Sort string
    let key = str.split("").sort().join("");

    // Create group if not exists
    if (!map[key]) {
      map[key] = [];
    }

    // Push original word
    map[key].push(str);
  }

  return Object.values(map);
};
```

---

# Dry Run

## Input

```js id="6o3n0v"
["eat", "tea", "tan", "ate", "nat", "bat"];
```

---

## Step 1

```js id="25y7eb"
"eat" -> "aet"
```

```js id="cujnhu"
map = {
  aet: ["eat"],
};
```

---

## Step 2

```js id="jtvv0n"
"tea" -> "aet"
```

```js id="9cpr3t"
map = {
  aet: ["eat", "tea"],
};
```

---

## Step 3

```js id="2a5bfj"
"tan" -> "ant"
```

```js id="fh6u26"
map = {
  aet: ["eat", "tea"],
  ant: ["tan"],
};
```

---

# Final

```js id="9lyj1m"
[["eat", "tea", "ate"], ["tan", "nat"], ["bat"]];
```

---

# Complexity

## Time Complexity

Sorting each string:

O(n \cdot k \log k)

Where:

- `n` = number of strings
- `k` = length of string

---

# Space Complexity

O(n \cdot k)

---

# Why This Works

All anagrams produce same sorted string.

Example:

```txt id="6tsu77"
eat -> aet
tea -> aet
ate -> aet
```

Same key → same group.

---

# Approach 3 → Frequency Count + HashMap (Optimized)

# Core Idea

Instead of sorting:

- count characters
- use frequency array as key

---

# Example

```txt id="9mcmde"
eat

a=1
e=1
t=1
```

Frequency array:

```txt id="mqej7y"
[1,0,0,0,1,0....1]
```

---

# Step 1 → Create Frequency Array

```js id="j1m0d6"
let count = new Array(26).fill(0);
```

---

# Step 2 → Count Characters

```js id="jlwmfp"
for (let ch of str) {
  count[ch.charCodeAt(0) - 97]++;
}
```

---

# ASCII Mapping

| Character | ASCII |
| --------- | ----- |
| a         | 97    |
| b         | 98    |
| c         | 99    |

---

# Why subtract 97?

```txt id="5vx35x"
a -> 97 - 97 = 0
b -> 98 - 97 = 1
c -> 99 - 97 = 2
```

Converts letters into indexes.

---

# Step 3 → Convert Array into String

```js id="j1xos4"
let key = count.join("#");
```

Example:

```txt id="n7qqf2"
"1#0#0#0#1#0#..."
```

---

# Why `join("#")`?

Without separator:

```js id="q5t1z5"
[1,11] -> "111"
[11,1] -> "111"
```

Collision ❌

---

# With separator

```js id="kmr33g"
"1#11";
"11#1";
```

Unique ✅

---

# Code

```js id="z5mswg"
var groupAnagrams = function (strs) {
  let map = new Map();

  for (let str of strs) {
    // Frequency array
    let count = new Array(26).fill(0);

    // Count chars
    for (let ch of str) {
      count[ch.charCodeAt(0) - 97]++;
    }

    // Convert to string
    let key = count.join("#");

    // Create group
    if (!map.has(key)) {
      map.set(key, []);
    }

    // Push string
    map.get(key).push(str);
  }

  return [...map.values()];
};
```

---

# Dry Run

## Input

```js id="pxb4mn"
["eat", "tea", "ate"];
```

---

## `"eat"`

Frequency:

```txt id="qqo7xg"
a=1
e=1
t=1
```

Key:

```txt id="x0d2iv"
1#0#0#0#1#...
```

---

## `"tea"`

Same frequency.

Same key.

So same group.

---

# Complexity

## Time Complexity

Character counting:

O(n \cdot k)

Better than sorting approach.

---

# Space Complexity

O(n \cdot k)

---

# Comparison Table

| Approach          | Time            | Space    | Easy?       |
| ----------------- | --------------- | -------- | ----------- |
| Brute Force       | O(n² × k log k) | O(n)     | Easy        |
| Sorting + HashMap | O(n × k log k)  | O(n × k) | Most Common |
| Frequency Count   | O(n × k)        | O(n × k) | Optimized   |

---

# Best Interview Answer

## If interviewer asks easiest solution

Use:

✅ Sorting + HashMap

---

## If interviewer asks optimized solution

Use:

✅ Frequency Count

---

# Important Interview Explanation

> “I used hashing to group words by a unique signature.”

### Sorting approach

> “All anagrams share same sorted form.”

Example:

```txt id="w3jj9n"
eat -> aet
tea -> aet
```

---

### Frequency count approach

> “Instead of sorting, I used character frequency encoding which reduces complexity from k log k to k per string.”

---

# Edge Cases

## Empty Input

```js id="j8n3f8"
[];
```

Output:

```js id="bvf5cy"
[];
```

---

## Empty Strings

```js id="6vx0q4"
["", ""];
```

Output:

```js id="h5cw5p"
[["", ""]];
```

---

## Single Word

```js id="56yqji"
["abc"];
```

Output:

```js id="db5l5x"
[["abc"]];
```

---

# Patterns Used

✅ HashMap
✅ Frequency Counting
✅ String Manipulation
✅ Array Encoding
✅ Grouping Pattern
✅ ASCII Mapping
