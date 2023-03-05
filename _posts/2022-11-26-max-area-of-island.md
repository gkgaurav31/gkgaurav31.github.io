---
layout: post
title: Max Area of Island
date: 2022-11-26 21:09 +0530
author: "Gaurav Kumar"
tags: "java leetcode graphs neetcode150"
categories: "neetcode150"
---

### Problem Description

You are given an m x n binary matrix grid. An island is a group of 1's (representing land) connected 4-directionally (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.  

The area of an island is the number of cells with a value 1 in the island.  

Return the maximum area of an island in grid. If there is no island, return 0.  

[leetcode](https://leetcode.com/problems/max-area-of-island/description/)

### Solution

We can apply BFS/DFS which should also return the number of 1s in the island found. Keep a track of max area each time we apply DFS. When we visit an island, change the 1s to 0s, so that we don't visit them again.

```java
class Solution {
    
    public int maxAreaOfIsland(int[][] grid) {

        int n = grid.length; int m = grid[0].length;

        //max area of island
        int count=0;

        //loop through all the cells
        for(int i=0; i<n; i++){
            for(int j=0; j<m; j++){

                //if it's water, skip
                if(grid[i][j] == 0) continue;

                //otherwise, use DFS and get the count of 1s in that island
                count = Math.max(count, dfs(grid, i, j));
            }
        }

        return count;

    }

    public int dfs(int[][] grid, int i, int j){
        
        //if i,j is out of bounds or grid[i][j] is water then return 0
        if(i<0 || j<0 || i>=grid.length || j>=grid[0].length || grid[i][j] == 0) return 0;

        //direction helper
        int[] x = {0,0,1,-1};
        int[] y = {1,-1,0,0};

        //the current cell is 1, so initialize count as 1
        int c = 1;
        //mark it visited
        grid[i][j] = 0;

        //go to each direction and call DFS recursively
        for(int k=0; k<4; k++){
            c += dfs(grid, i+x[k], j+y[k]);
        }

        return c;

    }

}
```
