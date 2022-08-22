---
layout: post
title: Count of Pairs with k Sum in Sorted Array
date: 2022-08-22 23:40 +0530
tags: "java arrays two_pointers"
categories: "two_pointers"
---

## Problem Description

Given a sorted array of distinct integers A and an integer B, find and return how many pair of integers ( A[i], A[j] ) such that i != j have sum equal to B.

### Solution

```java
public class Solution {

    public int solve(int[] A, int B) {

        int count=0;
        int i=0;
        int j=A.length-1;

        while(i<j){

            int sum = A[i] + A[j];

            if(sum == B){ //If we got the required sum, move both the pointers since there are distinct elements.
                count++;
                i++;
                j--;
            }else{          
                if(sum<B){//If currentSum is lesser, move index i to get a larger value
                    i++;
                }else{
                    j--;
                }
            }
        }
        return count;
    }
}
```
