# Sudoku Solver in Java: HashSet vs Bitmask Approach

This README covers two optimized Sudoku solving approaches in Java:

1. **Using HashSet**
2. **Using Bitmasking**

Both methods implement backtracking, but with different strategies to track used digits in rows, columns, and boxes.

---

## ✅ Problem Statement

Given a partially filled 9x9 Sudoku board, fill the board so that every row, column, and 3x3 box contains the digits 1 to 9 without repetition.

---

## 🚀 Approach 1: Using HashSet

### 🔧 Logic

* Use three arrays of `HashSet<Character>`:

    * `row[9]` to track digits in rows
    * `col[9]` to track digits in columns
    * `box[9]` to track digits in 3x3 boxes
* Traverse the board and populate these sets.
* During backtracking, check these sets to decide if a digit can be placed.

### 📌 Code Snippet

```java
class Solution {
    public void solveSudoku(char[][] board) {
       
        HashSet<Character>[] row = new HashSet[9];
        HashSet<Character>[] col = new HashSet[9];
        HashSet<Character>[] box = new HashSet[9]; 

        for(int i = 0; i < 9; i++){

            row[i] = new HashSet<>();
            col[i] = new HashSet<>();
            box[i] = new HashSet<>();

        }

        for(int i = 0; i < 9 ; i++){
            for(int j = 0; j < 9 ; j++){
                if(board[i][j] == '.') continue;
                row[i].add(board[i][j]);
                col[j].add(board[i][j]);
                int boxIndex = (i/3)*3 + j/3 ;
                box[boxIndex].add(board[i][j]);
            }
        }


        Backtrack(board , row , col , box, 0 , 0) ;
    }

    private Boolean Backtrack(char[][] board ,  HashSet<Character>[] row , HashSet<Character>[] col , HashSet<Character>[] box , int i , int j){
        if(i == 9) return true ;

        int nextRow = (j == 8 ) ? i+1 : i ;
        int nextCol = (j == 8 ) ? 0 : j+1 ;

        if(board[i][j] != '.') {
            return  Backtrack(board , row , col , box , nextRow , nextCol);
        }


        for(char c = '1' ; c <= '9' ; c++){

            int boxIndex = (i/3)*3 + j/3 ;

            if(row[i].contains(c) || col[j].contains(c) || box[boxIndex].contains(c)){
                continue;
            }

            row[i].add(c);
            col[j].add(c);
            box[boxIndex].add(c);
            board[i][j] = c;

            if(Backtrack(board , row , col , box , nextRow , nextCol)){
                return true;
            }

            board[i][j] = '.';
            row[i].remove(c);
            col[j].remove(c);
            box[boxIndex].remove(c);

        }

        return false;
    }

}
```

### ⚙️ Time and Space Complexity

* **Time Complexity:** O(1) average per add/contains/remove
* **Space Complexity:** O(81) = O(1), as there are only 81 cells

### ✅ Pros

* Easy to implement and understand
* Cleaner logic

### ❌ Cons

* Involves **boxing** of `char` to `Character`, which adds overhead . ------  ( Boxing / Unboxing) 
* Slightly slower due to `HashSet` object management

---

## 🚀 Approach 2: Using Bitmasking

### 🔧 Logic

* Use three arrays of `int[9]`:

    * Each `int` tracks digits using 9 bits (1–9)
* Use bitwise operations to set, clear, and check digits

### 📌 Bit Mapping

| Digit | Bit Position | Binary Mask |
| ----- | ------------ | ----------- |
| 1     | 0            | 000000001   |
| 2     | 1            | 000000010   |
| ...   | ...          | ...         |
| 9     | 8            | 100000000   |

### 📌 Code Snippet

```java
class Solution {
    public void solveSudoku(char[][] board) {

        int[] row = new int[9];
        int[] col = new int[9];
        int[] box = new int[9];

        for(int i = 0; i < 9 ; i++){
            for(int j = 0; j < 9 ; j++){
                char c = board[i][j] ;
                if( c == '.') continue;
                int digit = c - '0';
                int mask = 1 << (digit - 1) ;
                row[i] |= mask ;
                col[j] |= mask ;
                int boxIndex = (i/3)*3 + j/3 ;
                box[boxIndex] |= mask ;
            }
        }


        Backtrack(board , row , col , box, 0 , 0) ;
    }

    private Boolean Backtrack(char[][] board ,  int[] row , int[] col , int[] box , int i , int j){
        if(i == 9) return true ;

        int nextRow = (j == 8 ) ? i+1 : i ;
        int nextCol = (j == 8 ) ? 0 : j+1 ;

        if(board[i][j] != '.') {
            return  Backtrack(board , row , col , box , nextRow , nextCol);
        }


        for(char c = '1' ; c <= '9' ; c++){

            int boxIndex = (i/3)*3 + j/3 ;
            int digit = c - '0' ;
            int mask = 1 << ( digit - 1 ) ;
            if(( row[i] & mask ) != 0 || (col[j] & mask) != 0 || (box[boxIndex] & mask) != 0 ){
                continue;
            }

            row[i] |= mask ;
            col[j] |= mask ;
            box[boxIndex] |= mask ;
            board[i][j] = c;

            if(Backtrack(board , row , col , box , nextRow , nextCol)){
                return true;
            }

            board[i][j] = '.';
            row[i] ^= mask ;
            col[j] ^= mask ;
            box[boxIndex] ^= mask ;

        }

        return false;
    }

}
```

### ⚙️ Time and Space Complexity

* **Time Complexity:** O(1) per operation (bitwise)
* **Space Complexity:** O(27) = O(1), only 27 integers

### ✅ Pros

* **Faster** than `HashSet` due to primitive operations
* **Memory-efficient** (uses integers instead of objects)
* No garbage collection overhead

### ❌ Cons

* Slightly harder to read/maintain for beginners

---

## 🧠 Conversion Helpers

### ✅ Convert char to int

```java
char c = '2';
int num = c - '0'; // 2
```

### ✅ Convert int to char

```java
int num = 2;
char c = (char)(num + '0'); // '2'
```

---

## 🏁 Conclusion

Both approaches solve the problem effectively:

* **Use `HashSet`** if you prioritize readability
* **Use `Bitmasking`** if you need high performance and lower memory usage

> For competitive programming or large-scale Sudoku solving, **bitmasking is generally preferred**.

Feel free to modify and experiment with both to enhance your understanding of backtracking and optimization in Java!
