---
layout: post
title: Set Matrix Zeroes (InterviewBit)
date: 2023-02-12 23:53 +0530
author: "Gaurav Kumar"
tags: "java neetcode150 interviewbit mathematics geometry important"
categories: "neetcode150"
---

This question is part of [NeetCode150 series](https://neetcode.io/practice).

## Problem Description

Given an m x n integer matrix matrix, if an element is 0, set its entire row and column to 0's.
You must do it in place.

[leetcode](https://leetcode.com/problems/set-matrix-zeroes/description/)

### Solution

### APPROACH 1

Space Complexity: O(n+m)

```java
class Solution {

    public void setZeroes(int[][] matrix) {

        int n = matrix.length;
        int m = matrix[0].length;

        //we will use these two arrays to track if that particular row/column needs to be marked
        int[] rows = new int[n];
        int[] columns = new int[m];

        for(int i=0; i<n; i++){
            for(int j=0; j<m; j++){
                if(matrix[i][j] == 0){
                    rows[i] = 1;
                    columns[j] = 1;
                }
            }
        }

        for(int i=0; i<n; i++){
            for(int j=0; j<m; j++){
                if(rows[i] == 1 || columns[j] == 1) matrix[i][j] = 0;
            }
        }

    }

}
```

### APPROACH 2

[NeetCode YouTube](https://www.youtube.com/watch?v=T41rL0L3Pnw)

The above approach is not in-place as we used extra space.
Instead of maintaining two new arrays to track which rows and columns need to be updated, we can do it using first row and first column. The first cell is an overlap. So, we will use one extra variable for the first row.

```java
class Solution {

    public void setZeroes(int[][] matrix) {

        int n = matrix.length;
        int m = matrix[0].length;

        boolean firstRow = false;

        for(int i=0; i<n; i++){
            for(int j=0; j<m; j++){
                if(matrix[i][j] == 0){

                    matrix[0][j] = 0;

                    if(i == 0){
                        firstRow = true;
                    }else{
                        matrix[i][0] = 0;
                    }

                }
            }
        }

        for(int i=1; i<n; i++){
            for(int j=1; j<m; j++){
                if(matrix[i][0] == 0 || matrix[0][j] == 0) matrix[i][j] = 0;
            }
        }

        //first column
        if(matrix[0][0] == 0){
            for(int i=0; i<n; i++){
                matrix[i][0] = 0;
            }
        }

        //first row
        if(firstRow == true){
            for(int i=0; i<m; i++){
                matrix[0][i] = 0;
            }
        }

    }

}
```
