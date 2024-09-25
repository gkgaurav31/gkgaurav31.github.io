---
layout: post
title: Floyd Warshall (geeksforgeeks - SDE Sheet)
date: 2024-09-25 19:55 +0530
author: "Gaurav Kumar"
tags: "java graphs dynamic_programming geeksforgeeks sde-sheet important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

The problem is to find the shortest distances between every pair of vertices in a given edge-weighted directed graph. The graph is represented as an adjacency matrix of size n\*n. `Matrix[i][j]` denotes the weight of the edge from i to j. If `Matrix[i][j]=-1`, it means there is no edge from i to j.
Note : Modify the distances for every pair in-place.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/implementing-floyd-warshall2042/1?page=9)

## SOLUTION

[Good Explanation by takeUForward](https://www.youtube.com/watch?v=YbY8cVwWAvw)

```java
class Solution
{

    private static final int MAX = 1001;

    public void shortest_distance(int[][] matrix)
    {

        int n = matrix.length;

        // Replace all -1s in the matrix with MAX to represent no direct path between nodes.
        for(int i = 0; i < n; i++){
            for(int j = 0; j < n; j++){

                // Use MAX as a substitute for infinity.
                if(matrix[i][j] == -1)
                    matrix[i][j] = MAX;
            }
        }

        // Iterate over all pairs of nodes (i, j) and try to find a shorter path from i to j via an intermediate node k.
        for(int k = 0; k < n; k++){
            for(int i = 0; i < n; i++){
                for(int j = 0; j < n; j++){

                    // Update the distance from i to j by considering the distance from i to k
                    // plus the distance from k to j, and take the minimum.
                    matrix[i][j] = Math.min(matrix[i][j], matrix[i][k] + matrix[k][j]);
                }
            }
        }

        // Convert all MAX values back to -1 to indicate no path between nodes.
        for(int i = 0; i < n; i++){
            for(int j = 0; j < n; j++){
                if(matrix[i][j] >= MAX)
                    matrix[i][j] = -1;
            }
        }
    }
}
```
