# 445. Add Two Numbers II

## Problem Statement

You are given two non-empty linked lists representing two non-negative integers.

- Digits are stored in **forward order**
- Each node contains a single digit

Return the sum as a linked list.

---

## Example

```text id="o63ic7"
Input:
l1 = 7 → 2 → 4 → 3
l2 = 5 → 6 → 4

Output:
7 → 8 → 0 → 7
```

Explanation:

```text id="p6u8z9"
7243 + 564 = 7807
```

---

# Key Observation

Normal addition happens from:

```text id="6i7d9r"
RIGHT → LEFT
```

But linked list is stored as:

```text id="jlwmq0"
LEFT → RIGHT
```

So direct addition is difficult.

---

# Best Approach → Stack

We use two stacks.

Why?

Because stack removes elements from the end using:

```javascript id="sk8w9m"
pop();
```

This helps process digits from right to left.

---

# Step-by-Step Approach

## Step 1 → Store Digits into Stacks

### Input

```text id="98k3zc"
7 → 2 → 4 → 3
5 → 6 → 4
```

### Stacks

```text id="w8k4l1"
stack1 = [7,2,4,3]
stack2 = [5,6,4]
```

Top elements:

```text id="fj6r9q"
3 and 4
```

---

# Step 2 → Start Addition

## First Iteration

```text id="fxi3ot"
3 + 4 = 7
```

Result:

```text id="jlwmx1"
7
```

Carry:

```text id="9fj2rm"
0
```

---

## Second Iteration

```text id="4t2q8w"
4 + 6 = 10
```

Digit:

```text id="jlwmr7"
0
```

Carry:

```text id="4mk9zs"
1
```

Result:

```text id="m0p9xc"
0 → 7
```

---

## Third Iteration

```text id="jlwmh8"
2 + 5 + 1 = 8
```

Result:

```text id="jlwmc8"
8 → 0 → 7
```

---

## Fourth Iteration

```text id="t3z7fp"
7 + 0 = 7
```

Final Result:

```text id="0v7xq2"
7 → 8 → 0 → 7
```

---

# Important Logic

## Carry Calculation

```javascript id="3b9x7f"
carry = Math.floor(sum / 10);
```

Example:

```text id="h2m8w1"
sum = 18
```

\left\lfloor \frac{18}{10} \right\rfloor = 1

So:

```text id="x7k4p1"
carry = 1
```

---

## Current Digit

```javascript id="r6q1m8"
sum % 10;
```

Example:

18 \bmod 10 = 8

So current digit becomes:

```text id="d4v8k2"
8
```

---

# Most Important Part

```javascript id="u9m2q5"
node.next = head;
head = node;
```

This inserts the new node at the front.

---

## Example

Current list:

```text id="9x7p4m"
0 → 7
```

New node:

```text id="c1k8v3"
8
```

After insertion:

```text id="g5r2m9"
8 → 0 → 7
```

---

# Why Insert at Front?

Because we are processing digits from:

```text id="j7m1x4"
RIGHT → LEFT
```

But final linked list should be:

```text id="z4k8p2"
LEFT → RIGHT
```

So every new node is added at the front.

---

# JavaScript Solution

```javascript id="y3m7k1"
var addTwoNumbers = function (l1, l2) {
  let stack1 = [];
  let stack2 = [];

  // Store values into stacks
  while (l1) {
    stack1.push(l1.val);
    l1 = l1.next;
  }

  while (l2) {
    stack2.push(l2.val);
    l2 = l2.next;
  }

  let carry = 0;
  let head = null;

  while (stack1.length || stack2.length || carry) {
    let sum = carry;

    if (stack1.length) {
      sum += stack1.pop();
    }

    if (stack2.length) {
      sum += stack2.pop();
    }

    // Calculate carry
    carry = Math.floor(sum / 10);

    // Create current digit node
    let node = new ListNode(sum % 10);

    // Insert node at front
    node.next = head;
    head = node;
  }

  return head;
};
```

---

# Dry Run

## Initial

```text id="r9k4m1"
stack1 = [7,2,4,3]
stack2 = [5,6,4]
carry = 0
```

---

## Iteration 1

```text id="v2m8q5"
3 + 4 = 7
```

Result:

```text id="w6p1k9"
7
```

---

## Iteration 2

```text id="t4x7m2"
4 + 6 = 10
```

Digit:

```text id="p8k3v1"
0
```

Carry:

```text id="q5m9r4"
1
```

Result:

```text id="n7v2k8"
0 → 7
```

---

## Iteration 3

```text id="x1m6q9"
2 + 5 + 1 = 8
```

Result:

```text id="f4k8p3"
8 → 0 → 7
```

---

## Iteration 4

```text id="u7q2m5"
7 + 0 = 7
```

Final:

```text id="m3k9v1"
7 → 8 → 0 → 7
```

---

# Complexity Analysis

| Complexity | Value    |
| ---------- | -------- |
| Time       | O(n + m) |
| Space      | O(n + m) |

---

# ▶️ Why did you do it this way?

- Addition naturally happens from right to left.
- Linked list is stored in forward order.
- Stack helps reverse processing order easily.
- This avoids reversing the original linked lists.
- Code becomes simpler and cleaner.

---

# ▶️ What would you change if traffic grows 10x?

Current solution already works efficiently in:

```text id="m7q1k4"
O(n + m)
```

time.

But for extremely large linked lists:

- Stack memory usage can become high.
- We could optimize by:

  - Reversing linked lists in-place
  - Performing addition
  - Reversing result back

That reduces extra stack space.

Another optimization:

- Use recursion instead of explicit stacks if recursion depth is manageable.

---

# ▶️ Can you debug this live?

Yes.

Main debugging checkpoints:

## 1. Verify stack contents

```javascript id="y8m4q2"
console.log(stack1);
console.log(stack2);
```

Expected:

```text id="c6k1v8"
[7,2,4,3]
[5,6,4]
```

---

## 2. Check sum and carry

```javascript id="p3v7m1"
console.log(sum, carry);
```

---

## 3. Print current result list

```javascript id="t9k2m5"
console.log(head);
```

---

## 4. Common Bug

Wrong:

```javascript id="r5m8q1"
stack1.push(l1);
```

Correct:

```javascript id="v1k4p9"
stack1.push(l1.val);
```

Because we need numbers, not node objects.

---

# ▶️ What trade-offs did you make, and why?

## Trade-off

Used extra memory for stacks.

```text id="q7m3v1"
O(n + m) space
```

instead of modifying original lists.

---

## Why?

Advantages:

- Simpler logic
- Easy to debug
- Safer
- Original lists remain unchanged

Alternative approach:

- Reverse linked lists
- Add numbers
- Reverse again

That saves memory but increases pointer manipulation complexity.

---

# ▶️ How did you test this, and what edge cases did you consider?

## Test Case 1 → Normal Case

```text id="g4m8q2"
7243 + 564 = 7807
```

---

## Test Case 2 → Different Lengths

```text id="n9k1v5"
99 + 1 = 100
```

Output:

```text id="y6p3m8"
1 → 0 → 0
```

---

## Test Case 3 → Carry at End

```text id="v7m2q4"
999 + 999 = 1998
```

---

## Test Case 4 → One List Empty

```text id="r4k9p1"
0 + 123 = 123
```

---

## Test Case 5 → Single Digit

```text id="u1m7v3"
5 + 5 = 10
```

---

# Interview Explanation

> Since the digits are stored in forward order, direct addition is difficult because addition starts from the least significant digit.
>
> I used two stacks to reverse the processing order.
>
> Then I popped digits one by one, added them with carry, and inserted each new node at the front of the result list.
>
> This gives an efficient O(n + m) solution with clean implementation.
