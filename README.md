# Backtracking-3

## Problem1 
N Queens(https://leetcode.com/problems/n-queens/)
## solution 
class Solution {

    public List<List<String>> solveNQueens(int n) {
        List<List<String>> result = new ArrayList<>();
        char[][] board = new char[n][n];

        for (int i = 0; i < n; i++) {
            Arrays.fill(board[i], '.');
        }

        backtrack(board, 0, result);
        return result;
    }

    private void backtrack(char[][] board, int row, List<List<String>> result) {
        int n = board.length;

        if (row == n) {
            result.add(construct(board));
            return;
        }

        for (int col = 0; col < n; col++) {
            if (isSafe(board, row, col)) {
                board[row][col] = 'Q';
                backtrack(board, row + 1, result);
                board[row][col] = '.';
            }
        }
    }

    private boolean isSafe(char[][] board, int row, int col) {
        int n = board.length;

        // Check column
        for (int i = 0; i < row; i++) {
            if (board[i][col] == 'Q') return false;
        }

        // Check upper-left diagonal
        for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--) {
            if (board[i][j] == 'Q') return false;
        }

        // Check upper-right diagonal
        for (int i = row - 1, j = col + 1; i >= 0 && j < n; i--, j++) {
            if (board[i][j] == 'Q') return false;
        }

        return true;
    }

    private List<String> construct(char[][] board) {
        List<String> result = new ArrayList<>();
        for (char[] row : board) {
            result.add(new String(row));
        }
        return result;
    }
}


## Problem2
Word Search(https://leetcode.com/problems/word-search/)

## solution

class Solution {
    public boolean exist(char[][] board, String word) {
        int rows = board.length;
        int cols = board[0].length;

        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (backtrack(board, word, 0, i, j)) {
                    return true;
                }
            }
        }
        return false;
    }

    private boolean backtrack(char[][] board, String word, int index, int row, int col) {
        if (index == word.length()) {
            return true;  // Found the word
        }

        if (row < 0 || col < 0 || row >= board.length || col >= board[0].length || 
            board[row][col] != word.charAt(index)) {
            return false;  // Out of bounds or character does not match
        }

        // Mark the cell as visited (temporarily)
        char temp = board[row][col];
        board[row][col] = '#';  // Some invalid character

        // Explore all 4 directions (up, down, left, right)
        boolean found = 
            backtrack(board, word, index + 1, row + 1, col) ||
            backtrack(board, word, index + 1, row - 1, col) ||
            backtrack(board, word, index + 1, row, col + 1) ||
            backtrack(board, word, index + 1, row, col - 1);

        // Undo the mark (backtrack)
        board[row][col] = temp;

        return found;
    }
}
