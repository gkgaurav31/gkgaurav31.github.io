---
layout: post
title: Floyd Warshall
date: 2023-02-26 17:44 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks graphs important"
categories: "graphs"
---

### Problem Description

The problem is to find the shortest distances between every pair of vertices in a given edge-weighted directed graph. The graph is represented as an adjacency matrix of size n*n. ```Matrix[i][j]``` denotes the weight of the edge from i to j. If ```Matrix[i][j]=-1```, it means there is no edge from i to j.
Do it in-place.

[geeksforgeeks](https://practice.geeksforgeeks.org/problems/implementing-floyd-warshall2042/1?utm_source=gfg&utm_medium=article&utm_campaign=bottom_sticky_on_article)

### Solution

```java
class Solution
{
    public void shortest_distance(int[][] matrix)
    {
        
        int n = matrix.length;
        
        //init -> if the destination node is not reachable, set it to +infinite or more than the max value possible
        for(int i=0; i<n; i++){
            for(int j=0; j<n; j++){
                if(matrix[i][j] == -1) matrix[i][j] = 1001; // -1 <= matrix[ i ][ j ] <= 1000
            }
        }   
        
        for(int k=0; k<n; k++){
            for(int i=0; i<n; i++){
                for(int j=0; j<n; j++){
                    
                    //consider k as the intermediate node
                    //take Min(current cost from u to v , cost from u to k + cost from k to v)
                    matrix[i][j] = Math.min(matrix[i][j], matrix[i][k] + matrix[k][j]);                    
                }
            }   
        }
        
        for(int i=0; i<n; i++){
            for(int j=0; j<n; j++){
                if(matrix[i][j] >= 1001) matrix[i][j] = -1;
            }
        } 
        
    }
}
```
