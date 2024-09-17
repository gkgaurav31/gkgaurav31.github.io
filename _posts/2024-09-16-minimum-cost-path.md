---
layout: post
title: Minimum Cost Path (geeksforgeeks - SDE Sheet)
date: 2024-09-16 22:14 +0530
author: "Gaurav Kumar"
tags: "java heap graphs geeksforgeeks sde-sheet important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a square grid of size N, each cell of which contains an integer cost that represents a cost to traverse through that cell, we need to find a path from the top left cell to the bottom right cell by which the total cost incurred is minimum.
From the cell (i,j) we can go (i,j-1), (i, j+1), (i-1, j), (i+1, j).

[geeksforgeeks](https://www.geeksforgeeks.org/problems/minimum-cost-path3833/1?page=7)

## SOLUTION

### INCORRECT WAY

The following DP solution will not work since it assumes that we can move only in downwards and right directions.

```java
class Solution
{

    public int minimumCostPath(int[][] grid)
    {

        int n = grid.length;

        int[][] dp = new int[n][n];

        dp[0][0] = grid[0][0];

        for(int i=1; i<n; i++){
            dp[0][i] = dp[0][i-1] + grid[0][i];
            dp[i][0] = dp[i-1][0] + grid[i][0];
        }

        for(int i=1; i<n; i++){
            for(int j=1; j<n; j++){

                dp[i][j] = grid[i][j] + Math.min(dp[i-1][j], dp[i][j-1]);
            }
        }

        return dp[n-1][n-1];

    }

}
```

### RIGHT APPROACH (DIJKSTRA'S ALGORITHM)

```java
import java.util.*;

class Solution {

    public int minimumCostPath(int[][] grid) {

        int n = grid.length;

        // dp array to store the minimum cost to reach each cell
        int[][] dp = new int[n][n];

        // Initialize all cells in dp with infinity (or max value) as we want to minimize cost
        for (int i = 0; i < n; i++) {
            Arrays.fill(dp[i], Integer.MAX_VALUE);
        }

        // Priority queue (min-heap) to get the cell with the minimum cost at each step
        PriorityQueue<Cell> pq = new PriorityQueue<Cell>((a, b) -> a.cost - b.cost);

        //init:
        pq.add(new Cell(0, 0, grid[0][0]));
        dp[0][0] = grid[0][0];

        // Process cells until the priority queue is empty
        while (!pq.isEmpty()) {

            // Extract the cell with the minimum cost (current cell)
            Cell currentCell = pq.poll();

            int currentRow = currentCell.row;
            int currentColumn = currentCell.column;
            int currentCost = currentCell.cost;

            // direction helper
            int[] x = {0, 0, 1, -1};
            int[] y = {1, -1, 0, 0};

            // Check all four possible neighboring cells
            for (int k = 0; k < 4; k++) {

                int nextRow = currentRow + x[k];
                int nextColumn = currentColumn + y[k];

                // Check if the new cell is within grid bounds
                if (nextRow >= 0 && nextRow < n && nextColumn >= 0 && nextColumn < n) {

                    // Calculate the new cost to reach the neighboring cell
                    int newCost = currentCost + grid[nextRow][nextColumn];

                    // If the new cost is cheaper than the previously recorded cost, update it
                    if (newCost < dp[nextRow][nextColumn]) {

                        dp[nextRow][nextColumn] = newCost;

                        // Push the neighboring cell to the priority queue
                        pq.add(new Cell(nextRow, nextColumn, newCost));

                    }
                }
            }
        }

        // Return the minimum cost to reach the bottom-right corner (n-1, n-1)
        return dp[n-1][n-1];
    }
}

class Cell {

    int row;
    int column;
    int cost;

    Cell(int x, int y, int cost) {
        this.row = x;
        this.column = y;
        this.cost = cost;
    }
}
```
