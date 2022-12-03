---
layout: post
title: Search a 2D Matrix II
date: 2022-10-29 22:54 +0530
author: "Gaurav Kumar"
tags: "java  binary_search search"
categories: "search"
---

## Problem Description

Write an efficient algorithm that searches for a value target in an m x n integer matrix matrix. This matrix has the following properties:

- Integers in each row are sorted in ascending from left to right.
- Integers in each column are sorted in ascending from top to bottom.

[leetcode](https://leetcode.com/problems/search-a-2d-matrix-ii/)

### Solution

```java
class Solution {
    
    public boolean searchMatrix(int[][] matrix, int target) {
        
        int rows = matrix.length;
        int columns = matrix[0].length;
        
        //Start the search from top right corner
        int i=0, j=columns-1;
        
        while(i<rows && j>=0){
            
            int current = matrix[i][j];
            
            if(target == current) return true;

            //If target is more than current element, go down
            if(target > current) i++;

            //Else go left
            else j--;
            
        }
        
        return false;
        
    }
    
}
```
