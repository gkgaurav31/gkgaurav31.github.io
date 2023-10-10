---
layout: post
title: Remove Interval
date: 2023-10-10 20:50 +0530
author: "Gaurav Kumar"
tags: "java intervals leetcode leetcodealgo100"
categories: "intervals"
---

## PROBLEM DESCRIPTION

A set of real numbers can be represented as the union of several disjoint intervals, where each interval is in the form [a, b). A real number x is in the set if one of its intervals [a, b) contains x (i.e. a <= x < b).

You are given a sorted list of disjoint intervals intervals representing a set of real numbers as described above, where intervals[i] = [ai, bi] represents the interval [ai, bi). You are also given another interval toBeRemoved.

Return the set of real numbers with the interval toBeRemoved removed from intervals. In other words, return the set of real numbers such that every x in the set is in intervals but not in toBeRemoved. Your answer should be a sorted list of disjoint intervals as described above.

[leetcode](https://leetcode.com/problems/remove-interval/)

## SOLUTION

The tricky part if mainly to determine the different cases possible.

Loop through each interval:

- If the current interval starts after the interval to be removed, we can add it as is.
- If the current interval ends before the interval to be removed, we can add it as is.
- Otherwise, we know that there is some overlap. We mainly care of partial overlap at this point.
  - Left side: if the current interval starts before the interval to be removed -> add [start of current interval, start of interval to be removed]
  - right side: if the current intervals ends after the interval to be removed -> add [end of interval to be removed, end of current interval]

```java
class Solution {

    public List<List<Integer>> removeInterval(int[][] intervals, int[] toBeRemoved) {

        int n = intervals.length;

        List<List<Integer>> ans = new ArrayList<>();

        for(int i=0; i<n; i++){

            int[] interval = intervals[i];

            // if the current interval starts after toBeRemoved's end
            // OR
            // if the current intervals ends before toBeRemoved's start
            // we can simply add the current interval as is
            if(interval[0] > toBeRemoved[1] ||  interval[1] < toBeRemoved[0]){
                ans.add(Arrays.asList(interval[0], interval[1]));

            // otherwise, there is an overlap
            // we will include only if it's not a complete overlap
            }else{

                // if current interval start is before toBeRemoved's start
                if(interval[0] < toBeRemoved[0]){
                    ans.add(Arrays.asList(interval[0], toBeRemoved[0]));
                }

                // if current interval end is after toBeRemoved's end
                if(interval[1] > toBeRemoved[1]){
                    ans.add(Arrays.asList(toBeRemoved[1], interval[1]));
                }

                // if both are not true, it will be a complete overlap and we should not add anything for that interval

            }

        }

        return ans;

    }

}
```
