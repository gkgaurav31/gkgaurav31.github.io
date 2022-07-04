---
layout: post
title: Maximum Sum Sub-Matrix
date: 2022-07-04 22:05 +0530
author: "Gaurav Kumar"
tags: "java arrays important"
categories: "arrays"
---

## Problem Description

Given a 2D matrix, find the maximum possible sum of sub-matrices in that matrix.

### Solution

The brute force approach is to get all sub-matrices, calculate the sum and update the max-sum as we go on. Obviously the time complexity will be very high: 
O(N^2*M^2).
We can optimize it to O(N*M) which is the best possible time complexity possible for any matrix.

- First create a prefix sum matrix for the given matrix. To do this, first take the row-wise sum. Then do the same thing column wise.

```java
int[][]  pf = new int[n][m];

//Create prefix array

//Row wise
for(int i=0; i<n; i++){
    for(int j=0; j<m; j++){
        if(j == 0){
            pf[i][j] = matrix[i][j];
        }else{
            pf[i][j] = pf[i][j-1] + matrix[i][j];
        }

    }
}

//column wise
for(int j=0; j<m; j++){
    for(int i=0; i<n; i++){
    
        if(i == 0){
            pf[i][j] = pf[i][j];
        }else{
            pf[i][j] = pf[i-1][j] + pf[i][j];
        }
    }
}
```

- Create a method to get the sum of any given sub-matrix using the above created prefix sum matrix.

```java
public int getSumOfSubMatrix(int[][] pf, int topX, int topY, int bottomX, int bottomY){
    int sum = 0;

    if(topX==0 && topY == 0){
        sum = pf[bottomX][bottomY];
    }else if(topY==0){
        sum = pf[bottomX][bottomY]  - pf[topX-1][bottomY];
    }else if(topX == 0){
        sum = pf[bottomX][bottomY] - pf[bottomX][topY-1];
    }else{
        sum = pf[bottomX][bottomY] - pf[bottomX][topY-1] - pf[topX-1][bottomY] + pf[topX-1][topY-1];
    }

    return sum;
}
```

Now, the important part:
We will fix the rows first using two loops. Something like:

![snapshot]({{ site.baseurl }}/assets/img/max_sum_sub_matrix.jpg)

```java
for(int i=0; i<n; i++){ //start row
    for(int j=i; j<n; j++){ //end row
        //logic to get sum of the sub-matrix between startRow and EndRow. 
        //Then use Kadane's algorithm to get the maximum possible column wise.
        //Update the max sum if this is higher
    }
}
```

Complete Code:

```java
package com.gauk;

import java.util.*;

import java.util.*;

class Program {

    public int maximumSumSubmatrix(int[][] matrix) {

        int n = matrix.length;
        int m = matrix[0].length;

        int[][]  pf = new int[n][m];

        //Create prefix array

        //Row wise
        for(int i=0; i<n; i++){
            for(int j=0; j<m; j++){
                if(j == 0){
                    pf[i][j] = matrix[i][j];
                }else{
                    pf[i][j] = pf[i][j-1] + matrix[i][j];
                }

            }
        }

        //column wise
        for(int j=0; j<m; j++){
            for(int i=0; i<n; i++){

                if(i == 0){
                    pf[i][j] = pf[i][j];
                }else{
                    pf[i][j] = pf[i-1][j] + pf[i][j];
                }
            }
        }


        int maxSum = Integer.MIN_VALUE;

        for(int i=0; i<n; i++){ //start row
            for(int j=i; j<n; j++){ //end row

                int[] verticalSumArray = getColumnSumBetweenRows(pf, i, j);
                int sumByKardane = kadanesAlgorithm(verticalSumArray);
                maxSum = Math.max(sumByKardane, maxSum);
            }
        }

        return maxSum;

    }

    public int getSumOfSubMatrix(int[][] pf, int topX, int topY, int bottomX, int bottomY){
        int sum = 0;

        if(topX==0 && topY == 0){
            sum = pf[bottomX][bottomY];
        }else if(topY==0){
            sum = pf[bottomX][bottomY]  - pf[topX-1][bottomY];
        }else if(topX == 0){
            sum = pf[bottomX][bottomY] - pf[bottomX][topY-1];
        }else{
            sum = pf[bottomX][bottomY] - pf[bottomX][topY-1] - pf[topX-1][bottomY] + pf[topX-1][topY-1];
        }

        return sum;
    }

    public int[] getColumnSumBetweenRows(int[][] pf, int startRow, int endRow){

        int columns = pf[0].length;
        int[] vsum = new int[columns];

        for(int i=0; i<columns; i++){
            vsum[i] = getSumOfSubMatrix(pf, startRow, i, endRow, i);
        }

        return vsum;
    }

    public static int kadanesAlgorithm(int[] array) {

        int maxSum = Integer.MIN_VALUE;

        int maxSumTillHere = 0;

        for(int i=0; i<array.length; i++){
            maxSumTillHere = Math.max(maxSumTillHere + array[i], array[i]); //Either the sum will increase if we include the next number OR the next number has a larger value
            maxSum = Math.max(maxSum, maxSumTillHere);
        }

        return maxSum;

    }

}
```
