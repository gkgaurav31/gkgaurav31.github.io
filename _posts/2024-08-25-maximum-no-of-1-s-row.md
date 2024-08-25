---
layout: post
title: Maximum no of 1's row (geeksforgeeks - SDE Sheet)
date: 2024-08-25 20:23 +0530
author: "Gaurav Kumar"
tags: "java binary_search geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a boolean 2D array, where each row is sorted. Find the row with the maximum number of 1s.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/maximum-no-of-1s-row3027/1?page=2)

## SOLUTION

```java
class Sol
{
    public static int maxOnes (int mat[][], int n, int m)
    {

        int max = 0;
        int res = 0;

        for(int i=0; i<n; i++){

            int count = countOnes(mat[i], m);

            if(count > max){
                max = count;
                res = i;
            }

        }

        return res;

    }

    // get number of 1st using binary search
    public static int countOnes(int[] arr, int n){

        int l = 0;
        int r = n-1;

        int pos = -1;

        while(l <= r){

            int m = l + (r - l)/2;

            if(arr[m] == 1){
                pos = m;
                r = m-1;
            }else{
                l = m+1;
            }

        }

        if(pos == -1)
            return 0;

        return n - pos;

    }
}
```
