---
layout: post
title: Search for a Range (InterviewBit)
date: 2024-02-06 22:57 +0530
author: "Gaurav Kumar"
tags: "java binary_search"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given a sorted array of integers `A` (0 based index) of size `N`, find the starting and the ending position of a given integer `B` in array `A`.

Your algorithm's runtime complexity must be in the order of `O(log n)`.

Return an array of size 2, such that the first element = starting position of B in A and the second element = ending position of `B` in `A`, if `B` is not found in `A` return `[-1, -1]`.

## SOLUTION

We can use binary search two times - first to get the starting index and then to get the end index.

The main condition in binary search which will change is:

For start index:

```java
if(A[m] == B){
    idx = m;
    r = m-1;
}
```

For end index:

```java
if(A[m] == B){
    idx = m;
    l = m+1;
}
```

```java
public class Solution {
    // DO NOT MODIFY THE ARGUMENTS WITH "final" PREFIX. IT IS READ ONLY
    public int[] searchRange(final int[] A, int B) {

        int startIdx = startIndex(A, B);

        if(startIdx == -1) return new int[]{-1, -1};

        int endIdx = endIndex(A, B);

        return new int[]{startIdx, endIdx};

    }

    public int endIndex(int[] A, int B){

        int n = A.length;

        int l = 0;
        int r = n-1;

        int idx = -1;

        while(l<=r){

            int m = (l+r)/2;

            if(A[m] == B){
                idx = m;
                l = m+1;
            }else if(A[m] > B){
                r = m-1;
            }else{
                l = m+1;
            }

        }

        return idx;

    }

    public int startIndex(int[] A, int B){

        int n = A.length;

        int l = 0;
        int r = n-1;

        int idx = -1;

        while(l<=r){

            int m = (l+r)/2;

            if(A[m] == B){
                idx = m;
                r = m-1;
            }else if(A[m] > B){
                r = m-1;
            }else{
                l = m+1;
            }

        }

        return idx;

    }
}
```
