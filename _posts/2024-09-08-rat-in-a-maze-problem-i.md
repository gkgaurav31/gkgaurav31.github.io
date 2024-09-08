---
layout: post
title: Rat in a Maze Problem - I (geeksforgeeks - SDE Sheet)
date: 2024-09-08 17:13 +0530
author: "Gaurav Kumar"
tags: "java backtracking geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Consider a rat placed at (0, 0) in a square matrix mat of order n\* n. It has to reach the destination at (n - 1, n - 1). Find all possible paths that the rat can take to reach from source to destination. The directions in which the rat can move are 'U'(up), 'D'(down), 'L' (left), 'R' (right). Value 0 at a cell in the matrix represents that it is blocked and rat cannot move to it while value 1 at a cell in the matrix represents that rat can be travel through it.
Note: In a path, no cell can be visited more than one time. If the source cell is 0, the rat cannot move to any other cell. In case of no path, return an empty list. The driver will output "-1" automatically.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/rat-in-a-maze-problem/1?page=5)

## SOLUTION

Can we solved using standard DFS from position (0, 0).

```java
class Solution {
    public ArrayList<String> findPath(int[][] mat) {

        int n = mat.length;

        // Edge case: if the destination cell (n-1, n-1) is blocked (value 0), no path exists
        if (mat[n-1][n-1] == 0)
            return new ArrayList<String>();

        // Start DFS from the top-left corner (0, 0)
        return dfs(mat, new ArrayList<String>(), new ArrayList<>(), 0, 0);
    }

    public ArrayList<String> dfs(int[][] mat, ArrayList<String> current, ArrayList<String> paths, int i, int j) {

        int n = mat.length;

        // Base case: if the rat reaches the bottom-right corner (destination), add the current path to paths
        if (i == n-1 && j == n-1) {
            paths.add(String.join("", current));  // Join the list of moves into a single string
            return paths;
        }

        // Check boundary conditions and if the cell is blocked or already visited (value 0)
        if (i < 0 || j < 0 || i >= mat.length || j >= mat[0].length || mat[i][j] == 0) {
            return paths;
        }

        // Mark the current cell as visited by setting its value to 0 (avoid revisiting)
        mat[i][j] = 0;

        // Try moving 'Up' by reducing row index (i-1) and add 'U' to the current path
        current.add("U");
        dfs(mat, current, paths, i-1, j);
        current.remove(current.size() - 1);  // Backtrack to try other directions

        // Try moving 'Down' by increasing row index (i+1) and add 'D' to the current path
        current.add("D");
        dfs(mat, current, paths, i+1, j);
        current.remove(current.size() - 1);  // Backtrack

        // Try moving 'Left' by reducing column index (j-1) and add 'L' to the current path
        current.add("L");
        dfs(mat, current, paths, i, j-1);
        current.remove(current.size() - 1);  // Backtrack

        // Try moving 'Right' by increasing column index (j+1) and add 'R' to the current path
        current.add("R");
        dfs(mat, current, paths, i, j+1);
        current.remove(current.size() - 1);  // Backtrack

        // Unmark the current cell (backtracking step) by restoring its value to 1
        mat[i][j] = 1;

        // Return the list of all paths found so far
        return paths;
    }
}
```
