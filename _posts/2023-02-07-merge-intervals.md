---
layout: post
title: Merge Intervals
date: 2023-02-07 21:01 +0530
author: "Gaurav Kumar"
tags: "java leetcode intervals neetcode150"
categories: "neetcode150"
---

## PROBLEM DESCRIPTION

Given an array of intervals where intervals[i] = [starti, endi], merge all overlapping intervals, and return an array of the non-overlapping intervals that cover all the intervals in the input.

[leetcode](https://leetcode.com/problems/merge-intervals/description/)

### SOLUTION

```java
class Solution {

    public int[][] merge(int[][] intervals) {
        
        //sort based on start of the intervals
        Arrays.sort(intervals, (o1, o2) -> o1[0] > o2[0]?1: (o1[0] < o2[0]?-1:0) );

        List<int[]> list = new ArrayList<>();

        //init
        list.add(intervals[0]);

        //start merging from index 1
        for(int i=1; i<intervals.length; i++){
            
            //Get the end of last added interval
            int previousEnd = list.get(list.size()-1)[1];

            //If the previously added interval's end is more than or equal to the current interval's start, then we should merge them
            if(previousEnd >= intervals[i][0]){
                //to merge them, we can just update the end of previous interval which was added by max(previousEnd, currentEnd)
                list.get(list.size()-1)[1] = Math.max(previousEnd, intervals[i][1]);
            }else{
                //if they don't merge, add this new interval
                list.add(intervals[i]);
            }

        }

        //convert to int[][]
        int[][] ans = new int[list.size()][2];
        for(int i=0; i<list.size(); i++){
            ans[i] = list.get(i);
        }

        return ans;

    }

}
```
