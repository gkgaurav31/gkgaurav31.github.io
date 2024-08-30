---
layout: post
title: Shortest Source to Destination Path (geeksforgeeks - SDE Sheet)
date: 2024-08-30 16:30 +0530
author: "Gaurav Kumar"
tags: "java graphs geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a 2D binary matrix A(0-based index) of dimensions NxM. Find the minimum number of steps required to reach from (0,0) to (X, Y).
Note: You can only move left, right, up and down, and only through cells that contain 1.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/shortest-source-to-destination-path3544/1?page=4)

## SOLUTION

```java
class Solution {

    int shortestDistance(int n, int m, int A[][], int X, int Y) {

        int count = 0;

        // Edge case: Check if the starting point or the destination is blocked
        if(A[0][0] == 0 || A[X][Y] == 0)
            return -1;

        // Create a queue to store the coordinates for BFS
        Queue<int[]> q = new LinkedList<>();

        // Start BFS from the top-left corner (0,0)
        q.add(new int[]{0, 0});

        // Continue BFS until there are no more cells to explore
        while(!q.isEmpty()){

            // Get the number of elements in the current level of BFS
            int qSize = q.size();

            // Process each cell in the current level
            for(int i = 0; i < qSize; i++){

                int currentX = q.peek()[0];
                int currentY = q.peek()[1];

                q.poll();  // Remove the current cell from the queue

                // Mark the current cell as visited by setting it to 0
                A[currentX][currentY] = 0;

                // Return the number of steps if the destination is reached
                if(currentX == X && currentY == Y)
                    return count;

                // Arrays to move in four possible directions: right, left, down, up
                int[] x = {0, 0, 1, -1};
                int[] y = {1, -1, 0, 0};

                // Explore all four possible directions
                for(int k = 0; k < 4; k++){

                    int nextX = currentX + x[k];
                    int nextY = currentY + y[k];

                    // Check if the new position is within bounds and not yet visited
                    if(isValid(n, m, nextX, nextY) && A[nextX][nextY] == 1){

                        // Add the new position to the queue
                        q.add(new int[]{nextX, nextY});

                        // Mark the new position as visited
                        A[nextX][nextY] = 0;

                    }

                }

            }

            // We did not reach the destination, so need another step
            count++;

        }

        // Return -1 if the destination cannot be reached
        return -1;

    }

    // Helper function to check if a given cell (i, j) is within the bounds of the matrix
    boolean isValid(int n, int m, int i, int j){

        if(i < 0 || j < 0 || i >= n || j >= m)
            return false;

        return true;

    }

};
```
