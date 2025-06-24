## 🔍 Problem: Single Number III

**LeetCode Problem ID:** 260
**Difficulty:** Medium
**Link:** [LeetCode - Single Number III](https://leetcode.com/problems/single-number-iii)

---

### 🧾 Problem Statement

Given an integer array `nums`, in which exactly two elements appear only once and all the other elements appear exactly twice, return the two elements that appear only once. You can return the answer in any order.

**Example:**

```text
Input: nums = [1,2,1,3,2,5]
Output: [3,5]
```

---

### 💡 Intuition

The key observation is that if we XOR all the elements of the array, the result will be the XOR of the two unique numbers because duplicates cancel each other out in XOR.

Let the result of XOR of all numbers be `xor = a ^ b` (where `a` and `b` are the two unique numbers).

Now, at least one bit is set in `xor`, meaning that `a` and `b` differ at that bit. We find the **rightmost set bit** in `xor` and use it to divide the array elements into two groups:

* One group where this bit is **set**
* One group where this bit is **unset**

This way, `a` and `b` go into different groups, and XOR-ing each group separately will give us `a` and `b`.

---

### 🔄 Approach

1. XOR all numbers to get `xor = a ^ b`.
2. Find the rightmost set bit in `xor` (this bit is different between `a` and `b`).
3. Divide elements into two groups based on this bit.
4. XOR the elements in each group to find `a` and `b`.

---

### ✅ Code

```java
public class Solution {
    public int[] singleNumber(int[] nums) {
        int xor = 0;
        for (int num : nums) {
            xor ^= num;
        }

        int shift = 0;
        while ((xor & 1) == 0) {
            shift++;
            xor >>= 1;
        }

        int a = 0, b = 0;
        for (int num : nums) {
            if ((num & (1 << shift)) != 0) {
                a ^= num;
            } else {
                b ^= num;
            }
        }

        return new int[] {a, b};
    }
}
```

---

### ⏱️ Time Complexity

* **O(n)**: We traverse the array twice — once to compute XOR and once to divide and find the unique numbers.

### 🗃️ Space Complexity

* **O(1)**: We use a constant amount of extra space.

---

### 📘 Tags

`Bit Manipulation`, `Array`, `XOR`, `Divide and Conquer`

---

