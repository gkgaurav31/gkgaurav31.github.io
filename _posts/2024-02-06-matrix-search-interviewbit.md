---
layout: post
title: Matrix Search (InterviewBit)
date: 2024-02-06 22:48 +0530
author: "Gaurav Kumar"
tags: "java binary_search"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given a matrix of integers `A` of size `N x M` and an integer `B`. Write an efficient algorithm that searches for integer `B` in matrix `A`.

This matrix `A` has the following properties:

- Integers in each row are sorted from left to right.
- The first integer of each row is greater than or equal to the last integer of the previous row.

Return `1` if `B` is present in `A`, else return `0`.

NOTE: Rows are numbered from top to bottom, and columns are from left to right.

## SOLUTION

This problem is same as:
[Search a 2D Matrix]({% post_url 2022-10-28-search-a-2d-matrix %})

```java
public class Solution {

    public int searchMatrix(int[][] A, int B) {

        int n = A.length;
        int m = A[0].length;

        int l=0;
        int r=(n*m)-1;

        while(l<=r){

            int mid = (l+r)/2;

            int i = mid/m;
            int j = mid%m;

            if(A[i][j] == B){
                return 1;
            }else if(A[i][j] < B){
                l = mid+1;
            }else{
                r = mid-1;
            }

        }

        return 0;

    }

}
```
