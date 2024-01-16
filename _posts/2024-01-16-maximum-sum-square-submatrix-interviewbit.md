---
layout: post
title: Maximum Sum Square SubMatrix (InterviewBit)
date: 2024-01-16 21:37 +0530
author: "Gaurav Kumar"
tags: "java interviewbit arrays"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given a 2D integer matrix A of size `N x N` find a `B x B` submatrix where `B <= N` and `B >= 1`, such that sum of all the elements in submatrix is maximum.

[InterviewBit](https://www.interviewbit.com/problems/maximum-sum-square-submatrix/)

## SOLUTION

### BRUTE FORCE (TLE)

```java
public class Solution {

    public int solve(int[][] A, int B) {

        int n = A.length;

        // Calculate the prefix sum matrix
        int[][] pf = getPrefixSumMatrix(A);

        // Initialize the maximum sum to the minimum integer value
        int maxSum = Integer.MIN_VALUE;

        // We will try to form a submatrix of size B using [i, j] -> [topX, topY] and [k, w] -> [bottomX, bottomY]
        for(int i = 0; i < n; i++){ // Top-left corner's row index (topX)
            for(int j = 0; j < n; j++){ // Top-left corner's column index (topY)
                for(int k = 0; k < n; k++){ // Bottom-right corner's row index (bottomX)
                    for(int w = 0; w < n; w++){ // Bottom-right corner's column index (bottomY)

                        // Calculate the dimensions of the current submatrix
                        int length = w - j + 1;
                        int breadth = k - i + 1;

                        // Check if the dimensions match the required B x B
                        if(length == breadth && length == B)
                            // Update the maximum sum if the current submatrix has a higher sum
                            maxSum = Math.max(maxSum, getSubMatrixSum(pf, i, j, k, w));

                    }
                }
            }
        }

        return maxSum;
    }

    // Method to calculate the sum of elements in a submatrix using the prefix sum matrix
    public int getSubMatrixSum(int[][] pf, int topX, int topY, int bottomX, int bottomY){

        // Calculate the sum of the whole submatrix upto (bottomX, bottomY)
        int whole = pf[bottomX][bottomY];

        // Calculate the sum of the left part, if any
        int leftPart = topY > 0 ? pf[bottomX][topY - 1] : 0;

        // Calculate the sum of the top part, if any
        int topPart = topX > 0 ? pf[topX - 1][bottomY] : 0;

        // Calculate the sum of the overlapping part, if any
        int overlapPart = (topX > 0 && topY > 0) ? pf[topX - 1][topY - 1] : 0;

        // Calculate the final sum of the submatrix
        int sum = whole - leftPart - topPart + overlapPart;

        // Return the sum
        return sum;
    }

    // Method to calculate the prefix sum matrix of the given matrix A
    public int[][] getPrefixSumMatrix(int[][] A){

        int n = A.length;

        // Initialize the prefix sum matrix
        int[][] pf = new int[n][n];

        // Copy the values of the input matrix to the prefix sum matrix (we can do it in-place also if the given array can be modified)
        for(int i = 0; i < n; i++){
            for(int j = 0; j < n; j++){
                pf[i][j] = A[i][j];
            }
        }

        // Calculate row-wise cumulative sum
        for(int r = 0; r < n; r++){
            for(int c = 0; c < n; c++){
                if(c == 0){
                    continue;
                } else {
                    pf[r][c] = pf[r][c - 1] + pf[r][c];
                }
            }
        }

        // Calculate column-wise cumulative sum
        for(int c = 0; c < n; c++){
            for(int r = 0; r < n; r++){

                if(r == 0){
                    continue;
                } else {
                    pf[r][c] = pf[r - 1][c] + pf[r][c];
                }

            }
        }

        return pf;
    }
}
```

### OPTIMIZED (ACCEPTED)

Instead of calculating sum for each submatrix of size B x B one by one, we can use a different strategy.

First we set the start and end of the rows which will form the topX and bottomX co-ordinates of the sub-array. If the length from topX to bottomX is not equal to B, we can skip it.

If the length from topX to bottomX is B, we will use a temporary array to find the sum of elements column wise.

For example:

Given Arrays:

```text
1 2 3
4 5 6
7 8 9
```

Let's say B is 2. So, we need to find a 2 x 2 submatrix with maximum sum.

For one of the iterations, we can have topX = 0 and bottomX = 1 (length = 2).

For this range of rows, the sum of elements columns wise will be:

`[1+4 2+5 3+6]`

arr = `[5 7 9]`

Now, in this temporary array, we need to find a subarray of size B which has the maximum sum. For this we can make use of sliding windows technique! This sum will be the maximum sum which can be formed by choosing rows between 0 to 1 with length B=2. We will need to do this for all possible row combinations for which we will use two loops. The third loop will calculate `arr` array which has the sum of elements column wise. Finally, we will use a method to get the maximum sub subarray of size B. Update the answer, if it is >= maxSum.

```java
public class Solution {

    public int solve(int[][] A, int B) {

        int n = A.length;

        // Calculate the prefix sum matrix
        int[][] pf = getPrefixSumMatrix(A);

        // Initialize the maximum sum to the smallest possible integer value
        int maxSum = Integer.MIN_VALUE;

        // Array to store the column-wise sum
        int[] columnWiseSum = new int[n];

        // Select a range of rows and calculate the sum column-wise bounded by these rows using the prefix sum matrix
        for (int i = 0; i < n; i++) { // top of row

            for (int j = i; j < n; j++) { // bottom of row

                int length = j - i + 1;

                // Check if the selected range has the desired length B
                if (length != B) continue;

                // Update sum for each column in the selected range
                for (int col = 0; col < n; col++) {
                    columnWiseSum[col] = getSubMatrixSum(pf, i, col, j, col);
                }

                // Find the maximum sum subarray using sliding windows of size B (on the columnWiseSum array)
                int sum = maxSumSubArray(columnWiseSum, B);

                // Update the maximum sum
                maxSum = Math.max(maxSum, sum);

            }

        }

        return maxSum;

    }

    // Function to find the maximum sum subarray of size B using sliding windows
    public int maxSumSubArray(int[] A, int B) {

        int n = A.length;

        // Check if B is greater than the length of the array
        if (B > n) return 0;

        int s = 0;

        // Calculate the initial sum of the first B elements
        for (int i = 0; i < B; i++)
            s += A[i];

        int maxSum = s;

        // Iterate through the array and update the sum using sliding windows
        for (int i = B; i < n; i++) {
            s = s + A[i];
            s = s - A[i - B];
            maxSum = Math.max(maxSum, s);
        }

        return maxSum;

    }

    // Function to get the sum of a submatrix given the prefix sum matrix and the coordinates
    // Ref: https://gkgaurav31.github.io/posts/maximum-sum-sub-matrix/
    public int getSubMatrixSum(int[][] pf, int topX, int topY, int bottomX, int bottomY) {

        int whole = pf[bottomX][bottomY];

        int leftPart = topY > 0 ? pf[bottomX][topY - 1] : 0;
        int topPart = topX > 0 ? pf[topX - 1][bottomY] : 0;
        int overlapPart = (topX > 0 && topY > 0) ? pf[topX - 1][topY - 1] : 0;

        // Calculate the sum of the submatrix
        int sum = whole - leftPart - topPart + overlapPart;

        return sum;

    }

    // Function to calculate the prefix sum matrix from the given matrix
    public int[][] getPrefixSumMatrix(int[][] A) {

        int n = A.length;

        // Create a new matrix to store the prefix sum values
        int[][] pf = new int[n][n];

        // Copy the values from the input matrix to the prefix sum matrix
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                pf[i][j] = A[i][j];
            }
        }

        // Row-wise cumulative sum
        for (int r = 0; r < n; r++) {
            for (int c = 0; c < n; c++) {
                if (c == 0) {
                    continue;
                } else {
                    pf[r][c] = pf[r][c - 1] + pf[r][c];
                }
            }
        }

        // Column-wise cumulative sum
        for (int c = 0; c < n; c++) {
            for (int r = 0; r < n; r++) {
                if (r == 0) {
                    continue;
                } else {
                    pf[r][c] = pf[r - 1][c] + pf[r][c];
                }
            }
        }

        return pf;
    }
}
```
