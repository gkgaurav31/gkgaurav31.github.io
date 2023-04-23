---
layout: post
title: Transpose of a Matrix
date: 2023-04-23 14:52 +0530
tags: "java arrays matrix"
categories: "arrays"
---

### PROBLEM DESCRIPTION

Write a program to find the transpose of a square matrix of size N*N. Transpose of a matrix is obtained by changing rows to columns and columns to rows.

[geeksforgeeks](https://practice.geeksforgeeks.org/problems/transpose-of-matrix-1587115621/1)

### SOLUTION

The main observation is that, if we can every element A[i][j] with A[j][i], we get the transpose of the matrix.

```java
class Solution
{
    //Function to find transpose of a matrix.
    static void transpose(int matrix[][], int n)
    {
        
        for(int i=0; i<n; i++){
            for(int j=i; j<n; j++){
                swap(i, j, matrix);
            }
        }
        
    }
    
    static void swap(int i, int j, int[][] matrix){
        int temp = matrix[i][j];
        matrix[i][j] = matrix[j][i];
        matrix[j][i] = temp;
    }
}
```
