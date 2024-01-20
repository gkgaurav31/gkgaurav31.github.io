---
layout: post
title: Find Duplicate in Array (InterviewBit)
date: 2024-01-20 22:38 +0530
author: "Gaurav Kumar"
tags: "java interviewbit arrays"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given a read-only array of n + 1 integers between 1 and n, find one number that repeats in linear time using less than O(n) space and traversing the stream sequentially O(1) times.

If there are multiple possible answers ( like in the sample case ), output any one, if there is no duplicate, output -1

[InterviewBit](https://www.interviewbit.com/problems/find-duplicate-in-array/)

## SOLUTION

### APPROACH 1 - O(N) SPACE COMPLEXITY

```java
public class Solution {

    public int repeatedNumber(final int[] A) {

        int n = A.length;

        Set<Integer> set = new HashSet<>();

        for(int i=0; i<n; i++){
            if(set.contains(A[i])) return A[i];
            else set.add(A[i]);
        }

        return -1;
    }

}
```

### APPROACH 2 - MARK IN THE ARRAY

As per the constraints: `1 <= A[i] <= |A|`. So, if the element at any position is `X`, we can mark the index `(X-1)` as negative to mark that element as present. While iterating, if at any position we find that the value at index `(X-1)` is already -ve, that means it is a repeated number.

```java
public class Solution {

    public int repeatedNumber(final int[] A) {

        int n = A.length;

        for(int i=0; i<n; i++){

            int x = Math.abs(A[i]);

            if(A[x-1] < 0){
                return x;
            }else{
                A[x-1] = -A[x-1];
            }

        }

        return -1;

    }
}
```
