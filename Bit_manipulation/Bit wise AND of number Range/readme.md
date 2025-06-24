## 🔍 Problem: Bitwise AND of Numbers Range

**LeetCode Problem ID:** 201
**Difficulty:** Medium
**Link:** [LeetCode - Bitwise AND of Numbers Range](https://leetcode.com/problems/bitwise-and-of-numbers-range)

---

### 🧾 Problem Statement

Given two integers `left` and `right` that represent the range `[left, right]`, return the bitwise AND of all numbers in this range, inclusive.

**Example:**

```text
Input: left = 5, right = 7
Output: 4
Explanation:
5 -> 101
6 -> 110
7 -> 111
Bitwise AND = 100 (4)
```

---

### 💡 Intuition

When performing the bitwise AND operation over a range of numbers, the result will only keep the common leftmost bits. Any bit that differs between `left` and `right` (even by 1) will eventually become 0 because at least one number in the range will have a 0 in that bit position.

To find the common bits:

* Right shift both `left` and `right` until they become equal.
* Count how many shifts were made.
* Shift the result back to the left to get the final answer.

---

### 🔄 Approach

1. Initialize a `shift` counter.
2. While `left < right`, right shift both `left` and `right` by 1, and increment `shift`.
3. Once equal, shift `left` back left by `shift` and return the result.

---

### ✅ Code

```java
class Solution {
    public int rangeBitwiseAnd(int left, int right) {
        int shift = 0;

        while (left < right) {
            left >>= 1;
            right >>= 1;
            shift++;
        }

        return left << shift;
    }
}
```

---

### ⏱️ Time Complexity

* **O(1)** to **O(log N)**: where N is the difference between `left` and `right`. In the worst case, it runs for the number of bits in the integers (up to 32 for 32-bit integers).

### 🗃️ Space Complexity

* **O(1)**: No extra space is used.

---

### 📘 Tags

`Bit Manipulation`, `Math`

---

