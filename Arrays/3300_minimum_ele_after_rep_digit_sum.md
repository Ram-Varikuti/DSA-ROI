# Sum Digits of Numbers and Find Minimum

## Problem Statement

Given an array of integers `nums`:

- If a number has more than one digit, replace it with the sum of its digits.
- Keep single-digit numbers unchanged.
- Return the minimum value from the updated array.

---

## Example

### Input

```javascript
nums = [12, 5, 19];
```

### Transformation

```text
12 → 1 + 2 = 3
5  → 5
19 → 1 + 9 = 10
```

### Updated Array

```javascript
[3, 5, 10];
```

### Output

```javascript
3;
```

---

# Approach

1. Traverse the array.
2. Check whether the number has more than one digit.
3. If yes, calculate the sum of its digits.
4. Replace the original number with the digit sum.
5. After processing all elements, return the minimum element.

---

# Algorithm

```text
For each number in nums:
    If number > 9:
        Replace it with the sum of its digits

Return the minimum value from the updated array
```

---

# JavaScript Solution

```javascript
function minElement(nums) {
  for (let i = 0; i < nums.length; i++) {
    if (nums[i] > 9) {
      nums[i] = makeItSingle(nums[i]);
    }
  }

  return Math.min(...nums);

  function makeItSingle(num) {
    let sum = 0;

    for (let ch of String(num)) {
      sum += Number(ch);
    }

    return sum;
  }
}
```

---

# Helper Function Explanation

### Convert Number to String

```javascript
String(num);
```

Example:

```javascript
String(123); // "123"
```

---

### Iterate Through Each Digit

```javascript
for (let ch of String(num))
```

For:

```javascript
num = 123;
```

Iteration:

```text
'1'
'2'
'3'
```

---

### Convert Character to Number

```javascript
Number(ch);
```

Example:

```javascript
Number("1"); // 1
Number("5"); // 5
```

---

# Dry Run

## Input

```javascript
nums = [12, 5, 19];
```

---

### Iteration 1

```javascript
num = 12;
```

Digit Sum:

```text
1 + 2 = 3
```

Array:

```javascript
[3, 5, 19];
```

---

### Iteration 2

```javascript
num = 5;
```

Already a single-digit number.

Array:

```javascript
[3, 5, 19];
```

---

### Iteration 3

```javascript
num = 19;
```

Digit Sum:

```text
1 + 9 = 10
```

Array:

```javascript
[3, 5, 10];
```

---

### Find Minimum

```javascript
Math.min(...[3, 5, 10]);
```

Equivalent to:

```javascript
Math.min(3, 5, 10);
```

Output:

```javascript
3;
```

---

# Why Use Spread Operator?

### Incorrect

```javascript
Math.min(nums);
```

Output:

```javascript
NaN;
```

Reason:

`Math.min()` expects individual arguments, not an array.

---

### Correct

```javascript
Math.min(...nums);
```

Example:

```javascript
Math.min(...[3, 5, 10]);
```

becomes:

```javascript
Math.min(3, 5, 10);
```

Output:

```javascript
3;
```

---

# Complexity Analysis

### Time Complexity

Let:

- `n` = number of elements
- `d` = maximum number of digits in a number

Processing each number:

```text
O(d)
```

Processing the entire array:

```text
O(n × d)
```

---

### Space Complexity

No extra data structures are used.

```text
O(1)
```

---

# Key Interview Takeaways

### Convert Number to String

```javascript
String(num);
```

---

### Traverse Digits

```javascript
for (let ch of String(num))
```

---

### Convert Character to Number

```javascript
Number(ch);
```

---

### Find Minimum in Array

```javascript
Math.min(...nums);
```

---

### Common Mistake

❌ Wrong

```javascript
Math.min(nums);
```

✅ Correct

```javascript
Math.min(...nums);
```

---

# Pattern Used

- Array Traversal
- Digit Sum Calculation
- String Traversal
- Simulation

This pattern is commonly used in digit manipulation and array transformation problems.
