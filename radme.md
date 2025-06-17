# Valid Sudoku - Solutions and Explanation

This document includes two different approaches for validating a Sudoku board using Java. Both solutions solve the problem efficiently by ensuring that no digit from 1 to 9 appears more than once in any row, column, or 3x3 sub-grid.

---

## ✅ Problem Statement

Given a 9x9 Sudoku board, determine if it is valid. Only the filled cells need to be validated according to these rules:

* Each row must contain the digits 1-9 without repetition.
* Each column must contain the digits 1-9 without repetition.
* Each of the 9 3x3 sub-boxes of the grid must contain the digits 1-9 without repetition.

---

## ✅ Solution 1: Using 2D Integer Arrays

```java
class Solution {
    public boolean isValidSudoku(char[][] board) {
        if(board.length == 0) return false;

        int[][] rowCase = new int[9][9];
        int[][] colCase = new int[9][9];
        int[][] gridCase = new int[9][9];

        for(int row = 0; row < board.length; row++){
            for(int col = 0; col < board[0].length; col++){
                if(board[row][col] != '.'){
                    int number = board[row][col] - '0';
                    int k = row / 3 * 3 + col / 3;

                    if(rowCase[row][number-1] == 1 || colCase[col][number-1] == 1 || gridCase[k][number -1] == 1){
                        return false;
                    }

                    rowCase[row][number-1]++;
                    colCase[col][number-1]++;
                    gridCase[k][number -1]++;
                }
            }
        }
        return true;
    }
}
```

### 🔍 Explanation

* `rowCase[row][num-1]`, `colCase[col][num-1]`, and `gridCase[gridIndex][num-1]` track the frequency of digits 1-9.
* `number = board[row][col] - '0'` converts the char digit to an integer.
* `gridIndex = row/3 * 3 + col/3` determines which 3x3 sub-grid the cell belongs to.

### ⏱️ Time Complexity

* **O(1)** — As the board size is fixed (9x9), the loops run constant number of times.

### 🧠 Space Complexity

* **O(1)** — Uses 3 constant-size 9x9 matrices.

---

## ✅ Solution 2: Using HashSets

```java
class Solution {
    public boolean isValidSudoku(char[][] board) {
        HashSet<Character>[] row = new HashSet[9];
        HashSet<Character>[] col = new HashSet[9];
        HashSet<Character>[] box = new HashSet[9];

        for(int i = 0; i < 9; i++){
            row[i] = new HashSet<>();
            col[i] = new HashSet<>();
            box[i] = new HashSet<>();
        }

        for(int i = 0; i < 9; i++){
            for(int j = 0; j < 9; j++){
                char value = board[i][j];
                if(value == '.') continue;

                int boxIndex = (i/3) * 3 + j/3;
                if(row[i].contains(value) || col[j].contains(value) || box[boxIndex].contains(value)){
                    return false;
                }

                row[i].add(value);
                col[j].add(value);
                box[boxIndex].add(value);
            }
        }
        return true;
    }
}
```

### 🔍 Explanation

* Creates 3 arrays of HashSets to track characters seen in rows, columns, and boxes.
* `boxIndex = (i / 3) * 3 + j / 3` maps each cell to its 3x3 sub-grid.
* HashSets help in constant-time checking for duplicates.

### ⏱️ Time Complexity

* **O(1)** — Still constant time due to fixed board size.

### 🧠 Space Complexity

* **O(1)** — Uses 27 HashSets (3 arrays of size 9).

---

## 🆚 Comparison

| Feature          | Solution 1     | Solution 2                |
| ---------------- | -------------- | ------------------------- |
| Data Structure   | 2D Arrays      | HashSets                  |
| Lookup Time      | O(1)           | O(1) average              |
| Memory Usage     | Less flexible  | More readable and dynamic |
| Code Readability | More technical | More intuitive            |

---

## ✅ Final Notes

Both solutions are valid and efficient. Choose:

* **Solution 1** for compact and fast lookups.
* **Solution 2** for clarity and set-based thinking.

Great job maintaining a clean DSA repo! 💻📚
