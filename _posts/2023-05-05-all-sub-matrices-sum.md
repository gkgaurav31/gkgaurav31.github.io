---
layout: post
title: All Sub-Matrices Sum
date: 2023-05-05 22:38 +0530
author: "Gaurav Kumar"
tags: "java arrays geeksforgeeks matrix"
categories: "arrays"
---

### PROBLEM DESCRIPTION

Given a NxM 2-D matrix, the task to find the sum of all the submatrices.

[geeksforgeeks](https://www.geeksforgeeks.org/sum-of-all-submatrices-of-a-given-matrix/)

### SOLUTION

![snapshot]({{ site.baseurl }}/assets/img/all_submatrix_sum.png)

```java
import java.io.*;

class GFG {
 
 public static int matrixSum(int[][] matrix){
     
     int n = matrix.length;
     int m = matrix[0].length;
    
     int totalSum = 0;
     
     for(int i=0; i<n; i++){
         
         for(int j=0; j<m; j++){
             
             int topLeft = (i+1) * (j+1);
             int bottomRight = (n-i) * (m-j);
             
             int times = topLeft * bottomRight;
             
             int contribution = times * matrix[i][j];
             
             totalSum += contribution;
             
         }
     }
     
     return totalSum;
     
 }
 
}
```
