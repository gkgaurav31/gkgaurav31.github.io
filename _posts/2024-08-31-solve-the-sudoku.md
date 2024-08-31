---
layout: post
title: Solve the Sudoku (geeksforgeeks - SDE Sheet)
date: 2024-08-31 11:25 +0530
author: "Gaurav Kumar"
tags: "java backtracking geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given an incomplete Sudoku configuration in terms of a 9 x 9 2-D square matrix (grid[][]), the task is to find a solved Sudoku. For simplicity, you may assume that there will be only one unique solution.

A sudoku solution must satisfy all of the following rules:

- Each of the digits 1-9 must occur exactly once in each row.
- Each of the digits 1-9 must occur exactly once in each column.
- Each of the digits 1-9 must occur exactly once in each of the 9 3x3 sub-boxes of the grid.

Zeros in the grid indicates blanks, which are to be filled with some number between 1-9. You can not replace the element in the cell which is not blank.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/solve-the-sudoku-1587115621/1?page=4)

## SOLUTION

Iterate over the grid and look for positions which have not been filled. If position `pos` is `0`, it means it has not been filled yet. We can try filling it with values `[1, 9]` one by one iff it is valid. Let's say we want to check if 1 can be filled. So, we verify if there is no row which already has 1, no column already has 1 and that corresponding `3x3` sub square does not have one. If it is possible, then we put one and move to the next position recursively. If we have covered all positions, that means we found a valid sudoku grid. Otherwise, it will keep backtracking until all options are exhausted.

```java
class Solution
{
    static boolean SolveSudoku(int grid[][])
    {
        // check if sudoku can be solved, starting from position 0
        return solveSudokuHelper(grid, grid.length, 0);
    }

    static boolean solveSudokuHelper(int grid[][], int n, int pos){

        // If all cells have been processed, the Sudoku is solved.
        if(pos > 80)
            return true;

        // Calculate the row (i) and column (j) from pos
        int i = pos / 9;
        int j = pos % 9;

        // If the current cell is already filled, move to the next cell.
        if(grid[i][j] != 0)
            return solveSudokuHelper(grid, n, pos + 1);

        // Try filling the current empty cell with numbers from 1 to 9.
        for(int x = 1; x <= 9; x++){

            // Check if it's safe to place 'x' in the current cell (i, j).
            if(isSafeToFillWith(grid, n, i, j, x)){

                // Place 'x' in the cell.
                grid[i][j] = x;

                // Recursively attempt to fill the rest of the grid.
                if(solveSudokuHelper(grid, n, pos + 1))
                    return true;  // If successful, return true.

                // If placing 'x' didn't lead to a solution, backtrack
                grid[i][j] = 0;
            }
        }

        // If no number between 1 and 9 can be placed in this cell, return false.
        return false;
    }

    // Function to check if placing a number 'x' in position (row, column) is valid.
    static boolean isSafeToFillWith(int[][] grid, int n, int row, int column, int x){

        // Check if 'x' is not present in the current row.
        for(int c = 0; c < n; c++){
            if(grid[row][c] == x)
                return false;
        }

        // Check if 'x' is not present in the current column.
        for(int r = 0; r < n; r++){
            if(grid[r][column] == x)
                return false;
        }

        // Check if 'x' is not present in the 3x3 sub-grid containing the cell (row, column).
        int startRow = (row / 3) * 3;     // Top-left corner row of the 3x3 sub-grid.
        int startCol = (column / 3) * 3;  // Top-left corner column of the 3x3 sub-grid.

        for(int r = 0; r < 3; r++) {
            for(int c = 0; c < 3; c++) {
                if(grid[startRow + r][startCol + c] == x) {
                    return false;
                }
            }
        }

        // If 'x' is not found in the row, column, or 3x3 sub-grid, it's safe to place 'x'.
        return true;
    }

    static void printGrid (int grid[][])
    {
        for(int i = 0; i < grid.length; i++){
            for(int j = 0; j < grid[0].length; j++){
                System.out.print(grid[i][j] + " ");
            }
        }
    }
}
```
