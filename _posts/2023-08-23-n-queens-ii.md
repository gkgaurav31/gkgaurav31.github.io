---
layout: post
title: N-Queens II
date: 2023-08-23 22:32 +0530
author: "Gaurav Kumar"
tags: "java leetcode leetcode150 recursion backtracking"
categories: "backtracking"
---

## Problem Description

The n-queens puzzle is the problem of placing n queens on an n x n chessboard such that no two queens attack each other.

Given an integer n, return the number of distinct solutions to the n-queens puzzle.

[leetcode](https://leetcode.com/problems/n-queens-ii/)

## Solution

First part of the question:

[N-Queens I]({% post_url 2022-10-30-n-queens %})

Similar to N-Queens I, we go through each row one by one as we can place only a single queen in any given row. For each row, we try to check if we can place the queen on any of the columns. If a column is present, check for [row+1, 0] to [row+1, endOfColumn] for the next queen which can be placed. If it's not possible to place it, we can move to the next square. To check if it's possible to place a certain queen at any place, we mainly need to consider its cells above, cells in the same row, left diagonal and right diagonal.

```java
class Solution {

    public int totalNQueens(int n) {

        // Create an n x n chessboard represented by a 2D array.
        int[][] board = new int[n][n];

        // Start the recursive solving process and return the result.
        return check(board, 0, n);

    }

    // Recursive function to solve the N-Queens puzzle.
    public int check(int[][] board, int row, int n){

        // If we have successfully placed all queens (reached the end of the board), return 1 solution.
        if(row == n){
            return 1;
        }

        int total = 0; // Counter to keep track of the total number of solutions.

        // Iterate through each column in the current row and try placing a queen.
        for(int c=0; c<board[0].length; c++){

            if(canPlaceQueen(board, row, c)){

                // Place the queen in the current cell.
                board[row][c] = 1;

                // Recur for the next row and add the number of solutions obtained from it.
                total += check(board, row+1, n);

                // Backtrack: Remove the queen from the current cell to explore other possibilities.
                board[row][c] = 0; // Set the cell value back to 0

            }

        }

        return total;

    }

    // Function to check if it's safe to place a queen in the given cell.
    public boolean canPlaceQueen(int[][] board, int i, int j){

        // Check the current row for any queen conflict.
        // This part can actually be removed because we are any way ensure that we move to the next row after placing a queen in the current row.
        for(int x=0; x<board[0].length; x++){
            if(board[i][x] == 1) return false;
        }

        // Check the current column for any queen conflict.
        for(int x=i; x>=0; x--){
            if(board[x][j] == 1) return false;
        }

        // Check the left diagonal for any queen conflict.
        for(int x=i, y=j; x>=0 && y>=0; x--, y--){
            if(board[x][y] == 1) return false;
        }

        // Check the right diagonal for any queen conflict.
        for(int x=i, y=j; x>=0 && y<board[0].length; x--, y++){
            if(board[x][y] == 1) return false;
        }

        return true; // If no conflicts are found, it's safe to place a queen in the cell.

    }

}
```
