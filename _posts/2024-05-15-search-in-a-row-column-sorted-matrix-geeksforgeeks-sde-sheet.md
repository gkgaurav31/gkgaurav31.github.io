---
layout: post
title: Search in a row-column sorted Matrix (geeksforgeeks - SDE Sheet)
date: 2024-05-15 21:19 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks binary_search sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a matrix of size `n x m`, where every row and column is sorted in increasing order, and a number `x`. Find whether element `x` is present in the matrix or not.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/search-in-a-matrix-1587115621/1?page=2)

## SOLUTION

### APPROACH 1

Iterate row wise and use binary search in each row to look for element x. We can add a small optimization to do this search only if x is between first and last number in the current row.

```java
class Solution
{
static boolean search(int matrix[][], int n, int m, int x)
{

    for(int r=0; r<n; r++){

        if(x >= matrix[r][0] && x <= matrix[r][m-1]){
            if(binarysearch(matrix[r], x))
                return true;
        }

    }

    return false;

}

static boolean binarysearch(int[] arr, int x){

    int l = 0;
    int r = arr.length-1;

    while(l<=r){

        int m = (l+r)/2;

        if(arr[m] == x)
            return true;

        if(x > arr[m]){
            l = m+1;
        }else{
            r = m-1;
        }

    }

    return false;

}
}
```

### APPROACH 2

Check row wise - decide whether to check columns based on the range of numbers using the first and last number.
If it falls in that range, column also falls in that same range. Then x must be present in `matrix[r][c]` or not present at all.

This will have `O(n x m)` time complexity in the worst case.

```java
class Solution
{

    static boolean search(int matrix[][], int n, int m, int x)
    {

        // check if this row is possible
        for(int r=0; r<n; r++){

            if(x >= matrix[r][0] && x <= matrix[r][m-1]){

                // check if this column is possible
                for(int c=0; c<m; c++){

                    if(x >= matrix[0][c] && x <= matrix[n-1][c]){

                        // check this row and column
                        if(matrix[r][c] == x)
                            return true;

                    }

                }

            }

        }

        return false;

    }

}
```

### APPROACH 3 - OPTIMIZED | O(n+m)

The main idea is to start from top right corner.

- If x is same, return true
- If x is greater than element at current position, we go down (everything on left will be smaller)
- If x is smaller than element at current position, we go left (everything downwards will be larger)

We will reach out of bounds if the value is not present.

```java
    class Solution
    {
    //Function to search a given number in row-column sorted matrix.
    static boolean search(int matrix[][], int n, int m, int x)
    {

        int r = 0;
        int c = m-1;

        while(r < n && c >= 0){

            int current = matrix[r][c];

            if(current == x)
                return true;

            if(x > matrix[r][c]){
                r++;
            }else{
                c--;
            }

        }

        return false;

    }
    }
```
