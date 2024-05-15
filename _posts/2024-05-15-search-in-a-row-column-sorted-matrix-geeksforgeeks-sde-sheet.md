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
