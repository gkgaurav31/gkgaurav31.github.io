---
layout: post
title: N-Queen Problem (geeksforgeeks - SDE Sheet)
date: 2024-09-08 19:21 +0530
author: "Gaurav Kumar"
tags: "java backtracking geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

The n-queens puzzle is the problem of placing n queens on a (n×n) chessboard such that no two queens can attack each other.
Given an integer n, find all distinct solutions to the n-queens puzzle. Each solution contains distinct board configurations of the n-queens placement, where the solutions are a permutation of [1,2,3..n] in increasing order, here the number in the ith place denotes that the ith-column queen is placed in the row with that number. For eg below figure represents a chessboard [3 1 4 2].

[geeksforgeeks](https://www.geeksforgeeks.org/problems/n-queen-problem0315/1?page=5)

## SOLUTION

Each row can have at max 1 queen. So, in our recursive method, we keep incrementing only the row. For each row, we try to check if the queen can be placed between [0, N] one by one. If not, backtrack. Otherwise, place the queen and recursive do the same for next row. Once we have covered all the rows, we have a valid N queens setup, which we can add to the answer list.

```java
class Solution {

    public ArrayList<ArrayList<Integer>> nQueen(int n) {
        int[][] board = new int[n][n];
        return nQueenHelper(board, 0, new ArrayList<ArrayList<Integer>>());
    }

    public ArrayList<ArrayList<Integer>> nQueenHelper(int[][] board, int row, ArrayList<ArrayList<Integer>> res){

        int n = board.length;

        // If we've placed queens in all rows, store the solution
        if (row == n) {
            ArrayList<Integer> tempList = new ArrayList<>();

            for (int i = 0; i < n; i++) {
                for (int j = 0; j < n; j++) {
                    if (board[i][j] == 1) { // A queen is found at (i, j)
                        tempList.add(j + 1); // Store the 1-based column index
                        break; // Move to the next row after placing the queen
                    }
                }
            }
            // Add this solution to the result list
            res.add(tempList);
            return res;
        }

        // Try placing a queen in each column of the current row
        for (int column = 0; column < n; column++) {

            // Check if placing a queen at (row, column) is valid
            if (isValid(board, row, column)) {

                // Place the queen
                board[row][column] = 1;

                // Recursively place queen in the next row
                nQueenHelper(board, row + 1, res);

                // Backtrack by removing the queen
                board[row][column] = 0;

            }
        }

        return res;
    }

    // Function to check if placing a queen at (row, column) is valid
    public boolean isValid(int[][] board, int row, int column) {

        int n = board.length;

        // Check if there's any queen in the same column in previous rows
        for (int r = 0; r < row; r++) {
            if (board[r][column] == 1)
                return false;
        }

        // Check the left upper diagonal (top-left direction)
        int r = row;
        int c = column;
        while (r >= 0 && c >= 0) {
            if (board[r][c] == 1)
                return false;
            r--; c--; // Move diagonally up-left
        }

        // Check the right upper diagonal (top-right direction)
        r = row;
        c = column;
        while (r >= 0 && c < n) {
            if (board[r][c] == 1)
                return false;
            r--; c++; // Move diagonally up-right
        }

        // If no conflicts, return true
        return true;
    }
}
```
