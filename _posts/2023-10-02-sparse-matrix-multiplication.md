---
layout: post
title: Sparse Matrix Multiplication
date: 2023-10-02 19:33 +0530
author: "Gaurav Kumar"
tags: "java matrix leetcode leetcodealgo100"
categories: "matrix"
---

## PROBLEM DESCRIPTION

Given two sparse matrices mat1 of size m x k and mat2 of size k x n, return the result of mat1 x mat2. You may assume that multiplication is always possible.  
[leetcode](https://leetcode.com/problems/sparse-matrix-multiplication/)

## SOLUTION

```java
class Solution {

    public int[][] multiply(int[][] mat1, int[][] mat2) {

        int n1 = mat1.length;    // Number of rows in mat1
        int m1 = mat1[0].length; // Number of columns in mat1

        int n2 = mat2.length;    // Number of rows in mat2
        int m2 = mat2[0].length; // Number of columns in mat2

        // Create a result matrix with dimensions (n1 x m2)
        int[][] res = new int[n1][m2];

        // Initialize an index variable for iterating through the result matrix
        // We will use this to find the row/column for the next element to be inserted in the result
        // r = idx/m2
        // c = idx%m2
        // see: https://gkgaurav31.github.io/posts/search-a-2d-matrix/
        int idx = 0;

        // Iterate through rows of mat1
        for (int r1 = 0; r1 < n1; r1++) {

            // Iterate through columns of mat2
            for (int c2 = 0; c2 < m2; c2++) {

                // Initialize a variable to store the computed value
                int val = 0;

                // Perform the multiplication of corresponding elements from mat1 and mat2
                // by iterating through a common dimension (m1 or n2)
                for (int p = 0; p < m1; p++) { // We can also do: for (int p = 0; p < n2; p++)
                    val += mat1[r1][p] * mat2[p][c2];
                }

                // Calculate the indices (x, y) in the result matrix
                int x = idx / m2; // Row index
                int y = idx % m2; // Column index

                // Store the computed value in the result matrix at the corresponding indices
                res[x][y] = val;

                // Increment the index variable to move to the next position in the result matrix
                idx++;
            }
        }

        // Return the resulting matrix
        return res;
    }
}
```
