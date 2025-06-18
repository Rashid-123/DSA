# Leetcode 79: Word Search 

## ✅ Problem Statement

Given a 2D board and a word, check if the word exists in the grid. The word can be constructed from letters of sequentially adjacent cells (horizontally or vertically). Each cell may not be used more than once.

---

## 🚀 Approach: DFS with Backtracking

We perform a Depth First Search (DFS) starting from each cell in the grid to try to match the word character by character. Backtracking is used to explore all paths and revert changes after exploring a path.

---

## 🧱 Data Structures Used

### 1. `boolean[][] visited`

* **Purpose**: Keeps track of the cells visited in the current DFS path.
* **Why Needed**: To avoid using the same cell more than once.
* **Alternatives**: Instead of a separate grid, we could mark visited cells in-place using a special character (like `'#'`), and then unmark it after recursion.

---

## ⌛ Time and 📁 Space Complexity

### Time Complexity: `O(M * N * 4^L)`

Where:

* `M` = number of rows in the board
* `N` = number of columns in the board
* `L` = length of the word

**Explanation**:

* We explore each cell as a starting point (`M*N`).
* For each cell, we can branch into 4 directions (up, down, left, right), hence `4^L` possible paths (in the worst case).

### Space Complexity: `O(L)`

* Due to the recursion call stack of maximum depth `L` (length of the word).
* Plus `O(M*N)` space if using the `visited[][]` matrix explicitly.

---

## ⚖️ Comparison of `visited[][]` vs `'#'` Approach

### Using `visited[][]` (Your Code):

* Pros:

    * Clean separation of state (board stays unchanged).
    * Easier to understand for beginners.
* Cons:

    * Slightly more memory usage.

### Using `'#'` In-place Marking:

```java
char temp = board[row][col];
board[row][col] = '#'; // mark visited
// recursive calls
board[row][col] = temp; // unmark
```

* Pros:

    * Saves space.
* Cons:

    * Mutates the input board, may not be suitable if board must remain unchanged.

Both methods are valid. Choose based on your needs.

---

## 🔄 Backtracking Logic

1. Check if the current cell is within bounds.
2. Check if the current character matches the word.
3. Mark the cell as visited (in `visited[][]` or by overwriting).
4. Recursively check all 4 directions.
5. Unmark the cell (backtrack) after all calls.

---

## ✅ Code Summary (Using `visited[][]`)

```java
public boolean exist(char[][] board, String word) {
    int m = board.length, n = board[0].length;
    boolean[][] visited = new boolean[m][n];
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (Backtrack(board, word, i, j, 0, visited)) return true;
        }
    }
    return false;
}

private boolean Backtrack(char[][] board, String word, int row, int col, int index, boolean[][] visited) {
    if (index == word.length()) return true;
    if (row < 0 || col < 0 || row >= board.length || col >= board[0].length ||
        board[row][col] != word.charAt(index) || visited[row][col]) return false;

    visited[row][col] = true;
    boolean found = Backtrack(board, word, row+1, col, index+1, visited) ||
                    Backtrack(board, word, row-1, col, index+1, visited) ||
                    Backtrack(board, word, row, col+1, index+1, visited) ||
                    Backtrack(board, word, row, col-1, index+1, visited);
    visited[row][col] = false;
    return found;
}
```

---

## 💪 Alternate Code (Using '#' Marking In-place)

```java
public boolean exist(char[][] board, String word) {
    int m = board.length, n = board[0].length;
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (dfs(board, word, i, j, 0)) return true;
        }
    }
    return false;
}

private boolean dfs(char[][] board, String word, int row, int col, int index) {
    if (index == word.length()) return true;
    if (row < 0 || col < 0 || row >= board.length || col >= board[0].length ||
        board[row][col] != word.charAt(index)) return false;

    char temp = board[row][col];
    board[row][col] = '#';

    boolean found = dfs(board, word, row+1, col, index+1) ||
                    dfs(board, word, row-1, col, index+1) ||
                    dfs(board, word, row, col+1, index+1) ||
                    dfs(board, word, row, col-1, index+1);

    board[row][col] = temp; // unmark
    return found;
}
```

---

## 📄 Conclusion

* Backtracking is a powerful technique for path-search problems.
* Use `visited[][]` or character marking to avoid revisiting.
* Always backtrack by undoing your changes to maintain correctness of other search paths.

Happy Coding! ✨
