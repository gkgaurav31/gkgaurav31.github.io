---
layout: post
title: Rotate Image
date: 2023-02-12 16:34 +0530
author: "Gaurav Kumar"
tags: "java neetcode150 mathematics geometry important"
categories: "neetcode150"
---

This question is part of [NeetCode150 series](https://neetcode.io/practice).  

## Problem Description

You are given an n x n 2D matrix representing an image, rotate the image by 90 degrees (clockwise).

You have to rotate the image in-place, which means you have to modify the input 2D matrix directly. DO NOT allocate another 2D matrix and do the rotation.

[leetcode](https://leetcode.com/problems/rotate-image/description/)

### Solution

This problem can be solve by taking the transpose of the given matrix and then reversing each row one by one (reflection of the matrix).

[Transpose of a Matrix](https://www.youtube.com/watch?v=pqDZdKd1uLQ)

```java
class Solution {

    public void rotate(int[][] matrix) {

        int n = matrix.length;

        transpose(matrix, n);
        reflect(matrix, n);

    }

    //take either lower triangle and swap with upper triangle (we can do the other way round also)
    public void transpose(int[][] matrix, int n){

        for(int i=0; i<n; i++){
            for(int j=0; j<i; j++){
                int temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp;
            }
        }

    }

    //loop through each row and use two pointers to swap elemenets from front and back
    public void reflect(int[][] matrix, int n){

        for(int i=0; i<n; i++){

            int l = 0;
            int r = n-1;

            while(l<r){
                int temp = matrix[i][l];
                matrix[i][l] = matrix[i][r];
                matrix[i][r] = temp;

                l++; r--;

            }

        }

    }

}
```
