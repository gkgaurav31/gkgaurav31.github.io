---
layout: post
title: Find the number of islands (geeksforgeeks - SDE Sheet)
date: 2024-10-12 18:53 +0530
author: "Gaurav Kumar"
tags: "java graphs geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a grid of size n\*m (n is the number of rows and m is the number of columns in the grid) consisting of '0's (Water) and '1's(Land). Find the number of islands.

Note: An island is either surrounded by water or the boundary of a grid and is formed by connecting adjacent lands horizontally or vertically or diagonally i.e., in all 8 directions.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/find-the-number-of-islands/1?page=9)

## SOLUTION

We can solve this by using Depth-First Search (DFS) to count the number of islands in a 2D grid.

We go through each cell in the grid, and when we find a '1', we increase our island count and use DFS to mark all connected '1's as visited by changing them to '0'. This way, we ensure each island is counted only once.

```java
class Solution {

    public int numIslands(char[][] grid) {
        int n = grid.length;
        int m = grid[0].length;

        // Initialize island count
        int count = 0;

        // Iterate through each cell in the grid
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {

                // If the cell is land ('1'), we found a new island
                if (grid[i][j] == '1') {

                    count++;  // Increment island count

                    // Perform DFS to mark all adjacent land as visited
                    dfs(grid, i, j);

                }
            }
        }

        return count;
    }

    // Depth-First Search function to explore the island
    public void dfs(char[][] grid, int i, int j) {

        // Base case: check for out-of-bounds or if the cell is water ('0')
        if (i < 0 || j < 0 || i >= grid.length || j >= grid[0].length || grid[i][j] == '0') {
            return;
        }

        // Mark the current cell as visited by changing '1' to '0'
        grid[i][j] = '0';

        // direction helper
        int[] x = {0, 0, 1, -1, -1, 1, -1, 1};
        int[] y = {1, -1, 0, 0, 1, 1, -1, -1};

        // Explore all 8 directions
        for (int k = 0; k < 8; k++) {

            // Recursive DFS call for adjacent cells
            dfs(grid, i + x[k], j + y[k]);

        }
    }
}
```
