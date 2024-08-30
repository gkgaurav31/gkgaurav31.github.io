---
layout: post
title: Rotten Oranges (geeksforgeeks - SDE Sheet)
date: 2024-08-30 10:12 +0530
author: "Gaurav Kumar"
tags: "java graphs geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a grid of dimension nxm where each cell in the grid can have values 0, 1 or 2 which has the following meaning:

- 0 : Empty cell
- 1 : Cells have fresh oranges
- 2 : Cells have rotten oranges

We have to determine what is the earliest time after which all the oranges are rotten. A rotten orange at index [i,j] can rot other fresh orange at indexes [i-1,j], [i+1,j], [i,j-1], [i,j+1] (up, down, left and right) in unit time.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/rotten-oranges2536/1?page=4)

## SOLUTION

Can be solved using multi-source BFS.

```java
class Solution {

    public int orangesRotting(int[][] grid) {

        int n = grid.length;
        int m = grid[0].length;

        // to keep track of time to rot each orange
        int[][] time = new int[n][m];

        // queue for multi source BFS
        Queue<int[]> q = new LinkedList<>();

        // Traverse the grid to find all initially rotten oranges
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (grid[i][j] == 2) {
                    q.add(new int[]{i, j});
                }
            }
        }

        // BFS to rot adjacent fresh oranges
        while (!q.isEmpty()) {

            // Get the coordinates of the current rotten orange
            int i = q.peek()[0];
            int j = q.peek()[1];

            // Remove the rotten orange from the queue
            q.poll();

            // Arrays to represent the 4 possible directions (right, left, down, up)
            int[] x = {0, 0, 1, -1};
            int[] y = {1, -1, 0, 0};

            // Check all 4 possible directions
            for (int k = 0; k < 4; k++) {

                int nextX = i + x[k]; // Next row index
                int nextY = j + y[k]; // Next column index

                // check bounds
                if (nextX < 0 || nextY < 0 || nextX >= n || nextY >= m || grid[nextX][nextY] == 0 || grid[nextX][nextY] == 2) {
                    continue;
                }

                // If the next cell has a fresh orange, it gets rotted
                // It will eventually rot its neighboring oranges at later time
                // So, we need to add it to the queue
                q.add(new int[]{nextX, nextY});

                // Mark the orange as rotten in the grid
                // This is to avoid visiting this orange again
                grid[nextX][nextY] = 0;

                // Update the time to rot this orange
                // It will be 1 more than its already rotten neighbor
                time[nextX][nextY] = time[i][j] + 1;

            }
        }

        // Initialize the variable to track the maximum time taken to rot all oranges
        int maxTime = 0;

        // Traverse the grid to check if any fresh oranges are left and find the maximum time
        for (int i = 0; i < n; i++) {

            for (int j = 0; j < m; j++) {

                // If a fresh orange remains, return -1 (not all oranges can be rotted)
                if (grid[i][j] == 1) {
                    return -1;
                }

                // Update the maximum time taken to rot all oranges
                maxTime = Math.max(maxTime, time[i][j]);

            }
        }

        return maxTime;
    }
}
```
