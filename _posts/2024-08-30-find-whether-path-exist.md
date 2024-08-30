---
layout: post
title: Find whether path exist (geeksforgeeks - SDE Sheet)
date: 2024-08-30 18:04 +0530
author: "Gaurav Kumar"
tags: "java graphs geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a grid of size n\*n filled with 0, 1, 2, 3. Check whether there is a path possible from the source to destination. You can traverse up, down, right and left.
The description of cells is as follows:

- A value of cell 1 means Source.
- A value of cell 2 means Destination.
- A value of cell 3 means Blank cell.
- A value of cell 0 means Wall (blocked cell which we cannot traverse).

Note: There are only a single source and a single destination.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/find-whether-path-exist5238/1?page=4)

## SOLUTION

This can be solved using DFS.

```java
class Solution {

    public boolean is_Possible(int[][] grid) {

        int n = grid.length;
        int m = grid[0].length;

        // Initialize source and destination coordinates
        int si = -1, sj = -1, di = -1, dj = -1;

        // get the source and destination
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (grid[i][j] == 1) {
                    si = i;
                    sj = j;
                } else if (grid[i][j] == 2) {
                    di = i;
                    dj = j;
                }

                // Break if both source and destination are found
                if (si != -1 && di != -1) {
                    break;
                }
            }
        }

        // Perform depth-first search (DFS) to check if a path exists
        return dfs(grid, si, sj, di, dj);

    }

    public boolean dfs(int[][] grid, int si, int sj, int di, int dj) {

        // Check if the current cell is out of bounds or a wall
        if (di < 0 || dj < 0 || di >= grid.length || dj >= grid[0].length || grid[di][dj] == 0) {
            return false;
        }

        // If the current cell is the source cell, path is found
        if (di == si && dj == sj) {
            return true;
        }

        // Otherwise, mark the current cell as visited by setting it to 0
        grid[di][dj] = 0;

        // possible moves (right, left, down, up)
        int[] x = {0, 0, 1, -1};
        int[] y = {1, -1, 0, 0};

        // init:
        boolean reached = false;

        // Explore all four possible directions
        for (int k = 0; k < 4; k++) {

            int nextX = di + x[k];
            int nextY = dj + y[k];

            // Recursively perform DFS on the next cell
            // reached will be set to true if any of the moves returns true
            reached |= dfs(grid, si, sj, nextX, nextY);

        }

        return reached;
    }
}
```
