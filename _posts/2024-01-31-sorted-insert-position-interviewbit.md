---
layout: post
title: Sorted Insert Position (InterviewBit)
date: 2024-01-31 22:08 +0530
author: "Gaurav Kumar"
tags: "java binary_search"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given a sorted array `A` and a target value `B`, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You may assume no duplicates in the array.

## SOLUTION

We can use standard binary search to solve this.

```java
public class Solution {

    public int searchInsert(ArrayList<Integer> a, int b) {

        int l = 0;
        int r = a.size() - 1;

        while(l<=r){

            int m = (l+r)/2;

            // if value matches, return the current index m
            if(a.get(m) == b){
                return m;
            // if current value is lesser than b
            // we should look on the right side
            }else if(a.get(m) < b){
                l = m + 1;
            // if current value is more than b
            // we should look on the left side
            }else{
                r = m - 1;
            }

        }

        return l;

    }

}
```
