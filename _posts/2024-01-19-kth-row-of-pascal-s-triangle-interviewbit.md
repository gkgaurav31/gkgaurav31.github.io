---
layout: post
title: Kth Row of Pascal's Triangle (InterviewBit)
date: 2024-01-19 23:05 +0530
author: "Gaurav Kumar"
tags: "java interviewbit arrays"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given an index `k`, return the kth row of the Pascal's triangle.

Pascal's triangle: To generate `A[C]` in row R, sum up `A'[C]` and `A'[C-1]` from previous row `R - 1`.

Example:

Input : k = 3
Return : [1,3,3,1]

Note: k is 0 based. k = 0, corresponds to the row [1].

[InterviewBit](https://www.interviewbit.com/problems/segregate-0s-and-1s-in-an-array/)

Note: Could you optimize your algorithm to use only O(k) extra space?

## SOLUTION

`A[row][col] = A[row-1][col-1] + A[row-1][col]`

The `col-1` value can become -ve, so we need to handle that.

```text
1
1 1
1 2 1
1 3 3 1
```

```java
public class Solution {

    public int[] getRow(int A) {

        int n = A+1;

        int[][] arr = new int[n][n]; //last row (ans) will be of length n

        arr[0][0] = 1;  // first row starts with only 1

        for(int r=1; r<n; r++){

            for(int c=0; c<=r; c++){

                // get the value in previous row, previous column
                int val1 = (c-1 < 0)?0:arr[r-1][c-1];

                // get the value in previous row, same column
                int val2 = arr[r-1][c];

                arr[r][c] = val1 + val2;

            }

        }

        return arr[n-1]; //last row is our answer

    }

}
```

### SPACE OPTIMIZATION

At any step, we just need the previous row values to calculate the next row. So, instead of taking `n x n` array, we can just use `2 x n` array. One row will store the previous row answer, and the other row will calculate based on that row.

`Trick`: We can easily switch between the rows using %2!

```java
public class Solution {

    public int[] getRow(int A) {

        int n = A+1;

        int[][] arr = new int[2][n];

        arr[0][0] = 1;

        for(int r=1; r<n; r++){
            for(int c=0; c<=r; c++){

                int val1 = ((c-1) < 0)?0:arr[(r-1)%2][(c-1)];
                int val2 = arr[(r-1)%2][c];

                arr[r%2][c] = val1 + val2;

            }
        }

        return arr[(n-1)%2];


    }

}
```
