---
layout: post
title: Number of Islands
date: 2022-11-26 01:01 +0530
author: "Gaurav Kumar"
tags: "java leetcode graphs"
categories: "graphs"
---

### Problem Description

Given an m x n 2D binary grid grid which represents a map of '1's (land) and '0's (water), return the number of islands.

An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.
[leetcode](https://leetcode.com/problems/number-of-islands/description/)

### Solution

If we carefully observe, the numbers of lands are actually the "number of connected componenets" in the matrix. So, all we need to do is really apply DFS on the given matrix and each time we apply DFS, we increase the count of number of components. The interesting thing here is that we do not really require an adjacency list. An adjacency list contains information about the node's neighbours. Here, we already know that the neighbors can be either top, down, left or right. If these directions have water (char '0'), then that means, we cannot take that path so we skip it. While applying DFS, we would need a way to also "visit" the nodes (land marked '1'). We can do that by changing its value to 0, because we don't visit water in this question. Note that, with this approach, we will be modifying the input matrix.

```java
class Solution {

    public int numIslands(char[][] grid) {
        
        int n = grid.length;
        int m = grid[0].length;

        int count = 0;

        for(int i=0; i<n; i++){
            for(int j=0; j<m; j++){

                if(grid[i][j] == '0') continue;

                dfs(i, j, grid);
                count++;

            }
        }

        return count;

    }

    public void dfs(int i, int j, char[][] grid){

        //We will use this for the 4 direction in which we can go from grid[i][j]
        //[i][j-1], [i][j+1], [i-1][j], [i+1][j]
        int[] x = {0, 0, 1, -1};
        int[] y = {-1, 1, 0, 0};

        for(int k=0; k<4; k++){
            
            //Possible next positions
            int a = i+x[k];
            int b = j+y[k];

            //Check if they are within the allowed bounds and it's not water
            if(a >= 0 && b>=0 && a<grid.length && b<grid[0].length && grid[a][b] != '0'){

                //update it to water so that we don't go there again
                grid[a][b] = '0';

                //apply DFS from that node
                dfs(a, b, grid);
            }

        }

    }

}
```

### Another way to code

```java
public void dfs(int i, int j, char[][] grid){

    if(i<0 || j<0 || i>=grid.length || j>=grid[0].length || grid[i][j] == '0') return;

    int[] x = {0, 0, 1, -1};
    int[] y = {-1, 1, 0, 0};

    grid[i][j] = '0';

    for(int k=0; k<4; k++){
        dfs(i+x[k], j+y[k], grid);   
    }

}
```
