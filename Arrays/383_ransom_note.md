# 📌 LeetCode 383 - Ransom Note

## Problem Statement

Given two strings `ransomNote` and `magazine`, return `true` if `ransomNote` can be constructed using the letters from `magazine`, otherwise return `false`.

### Rule

- Each character in `magazine` can be used **only once**.
- All characters are lowercase English letters.

---

## Examples

### Example 1

```js
ransomNote = "a";
magazine = "b";
```

Output:

```js
false;
```

---

### Example 2

```js
ransomNote = "aa";
magazine = "ab";
```

Output:

```js
false;
```

---

### Example 3

```js
ransomNote = "aa";
magazine = "aab";
```

Output:

```js
true;
```

---

# Pattern

```txt
✔ Frequency Counting
✔ HashMap
✔ Character Counting
✔ Array Frequency
```

---

# Approach 1: Brute Force

## Idea

For every character in `ransomNote`:

1. Search for it in `magazine`
2. If found, remove it
3. If not found, return `false`

---

## Code

```js
var canConstruct = function (ransomNote, magazine) {
  magazine = magazine.split("");

  for (let ch of ransomNote) {
    let idx = magazine.indexOf(ch);

    if (idx === -1) {
      return false;
    }

    magazine.splice(idx, 1);
  }

  return true;
};
```

---

## Dry Run

### Input

```js
ransomNote = "aa";
magazine = "aab";
```

Convert magazine into array:

```js
["a", "a", "b"];
```

### First 'a'

```js
indexOf('a') → 0
```

Remove it:

```js
["a", "b"];
```

### Second 'a'

```js
indexOf('a') → 0
```

Remove it:

```js
["b"];
```

Loop completed.

```js
return true;
```

---

## Complexity

### Time

```txt
O(n × m)
```

### Space

```txt
O(m)
```

---

# Approach 2: HashMap Frequency Count

## Idea

Count how many times each character appears in `magazine`.

While processing `ransomNote`:

- If character is unavailable → return `false`
- Otherwise use one occurrence

---

## Code

```js
var canConstruct = function (ransomNote, magazine) {
  let freq = {};

  for (let ch of magazine) {
    freq[ch] = (freq[ch] || 0) + 1;
  }

  for (let ch of ransomNote) {
    if (!freq[ch]) {
      return false;
    }

    freq[ch]--;
  }

  return true;
};
```

---

## Understanding `if (!freq[ch])`

This handles two cases:

### Case 1: Character doesn't exist

```js
freq["c"] = undefined;
```

```js
!undefined === true;
```

---

### Case 2: Character exhausted

```js
freq["a"] = 0;
```

```js
!0 === true;
```

---

Therefore:

```js
if (!freq[ch]) {
  return false;
}
```

means:

```txt
Character not available
OR
Character count finished
```

---

## Understanding `freq[ch]--`

Suppose:

```js
magazine = "aab";
```

Frequency map:

```js
{
    a: 2,
    b: 1
}
```

Use first `'a'`:

```js
freq["a"]--;
```

Now:

```js
{
    a: 1,
    b: 1
}
```

Use second `'a'`:

```js
freq["a"]--;
```

Now:

```js
{
    a: 0,
    b: 1
}
```

Meaning:

```txt
One occurrence has been consumed.
```

Without decrementing:

```txt
The same character could be reused multiple times.
```

---

## Dry Run

### Input

```js
ransomNote = "aa";
magazine = "aab";
```

Frequency Map:

```js
{
    a: 2,
    b: 1
}
```

### First 'a'

```js
freq["a"] = 2;
```

Consume:

```js
freq["a"]--;
```

Now:

```js
a = 1;
```

---

### Second 'a'

```js
freq["a"] = 1;
```

Consume:

```js
freq["a"]--;
```

Now:

```js
a = 0;
```

Finished.

```js
return true;
```

---

## Complexity

### Time

```txt
O(n + m)
```

### Space

```txt
O(k)
```

Where:

```txt
k = number of distinct characters
```

---

# Approach 3: Optimal (Array of Size 26)

## Key Observation

Strings contain only:

```txt
a-z
```

There are exactly:

```txt
26 letters
```

Instead of a HashMap, use a fixed-size array.

---

## Character Mapping

```txt
a → 0
b → 1
c → 2
...
z → 25
```

Using:

```js
ch.charCodeAt(0) - 97;
```

---

### Examples

```js
"a".charCodeAt(0) - 97;
```

```js
97 - 97 = 0
```

---

```js
"b".charCodeAt(0) - 97;
```

```js
98 - 97 = 1
```

---

```js
"z".charCodeAt(0) - 97;
```

```js
122 - 97 = 25
```

---

## Code

```js
var canConstruct = function (ransomNote, magazine) {
  let count = new Array(26).fill(0);

  for (let ch of magazine) {
    count[ch.charCodeAt(0) - 97]++;
  }

  for (let ch of ransomNote) {
    let idx = ch.charCodeAt(0) - 97;

    if (count[idx] === 0) {
      return false;
    }

    count[idx]--;
  }

  return true;
};
```

---

## Dry Run

### Input

```js
ransomNote = "aa";
magazine = "aab";
```

### Initial Array

```js
[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0];
```

---

### Process Magazine

#### First 'a'

```js
count[0]++;
```

```txt
a = 1
```

#### Second 'a'

```js
count[0]++;
```

```txt
a = 2
```

#### 'b'

```js
count[1]++;
```

```txt
b = 1
```

Current state:

```txt
a = 2
b = 1
```

---

### Process Ransom Note

#### First 'a'

```js
count[0] = 2;
```

Consume:

```js
count[0]--;
```

```txt
a = 1
```

---

#### Second 'a'

```js
count[0] = 1;
```

Consume:

```js
count[0]--;
```

```txt
a = 0
```

Finished.

```js
return true;
```

---

## Complexity

### Time

```txt
O(n + m)
```

### Space

```txt
O(1)
```

Because the array size is always:

```txt
26
```

---

# Interview Discussion

## Why HashMap?

Because we need:

```txt
Character → Frequency
```

with:

```txt
O(1) lookup
```

---

## Why Array Instead of HashMap?

Since only lowercase letters are present:

```txt
26 fixed characters
```

Array access is faster and uses constant space.

---

## Why Decrement Count?

Problem says:

```txt
Each letter can be used only once.
```

After using a character:

```js
count[idx]--;
```

reduces the remaining availability.

---

## What If Traffic Grows 10x?

Current optimal solution:

```txt
O(n + m)
```

Still scales efficiently because:

- Single pass to count frequencies
- Single pass to consume characters
- No nested loops

---

## Debugging Approach

Print frequency array:

```js
console.log(count);
```

Print current character:

```js
console.log(ch, count[idx]);
```

Check:

- Wrong index calculation
- Missing character
- Count becoming zero unexpectedly

---

# Edge Cases

### Empty ransomNote

```js
ransomNote = "";
magazine = "abc";
```

Output:

```js
true;
```

---

### Empty magazine

```js
ransomNote = "a";
magazine = "";
```

Output:

```js
false;
```

---

### More occurrences required

```js
ransomNote = "aaa";
magazine = "aa";
```

Output:

```js
false;
```

---

### Exact Match

```js
ransomNote = "abc";
magazine = "abc";
```

Output:

```js
true;
```

---

# Key Takeaways

```txt
1. Count available characters from magazine.
2. Consume characters required by ransomNote.
3. If any count becomes unavailable, return false.
4. HashMap solution → O(n + m).
5. Array(26) solution → O(n + m) time and O(1) space.
6. Classic Frequency Counting pattern problem.
7. Decrement count after using a character.
8. Use Array(26) when input contains only lowercase English letters.
```
