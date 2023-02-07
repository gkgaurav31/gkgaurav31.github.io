---
layout: post
title: Non-overlapping Intervals
date: 2023-02-07 23:40 +0530
author: "Gaurav Kumar"
tags: "java leetcode intervals greedy neetcode150 important"
categories: "neetcode150"
---

## PROBLEM DESCRIPTION

Given an array of intervals intervals where intervals[i] = [starti, endi], return the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping.

[leetcode](https://leetcode.com/problems/non-overlapping-intervals/description/)

### SOLUTION

[NeetCode YouTube](https://www.youtube.com/watch?v=nONCGxWoUfM)

This problem is actually quick tricky. We need to use a greedy approach to solve it. First thing which is obviously needed is to sort the intervals based on start. Save the end of first interval in "previousEnd". Now, check the other interval one by one. If the next interval is not overlapping, we can simply update "previousEnd" which will be the farthest end we have got in the intervals. But, if there is an overlap then we have to think of two options. Should we remove the current interval or previous one? We should be removing the interval which ENDS FIRST. If we do that, then we are basically reducing the chance of overlapping in the future for other intervals, which is what we desire. Keep following this process and keep a count whenever there is an overlap case.

```java
class Solution {

    public int eraseOverlapIntervals(int[][] intervals) {

        //sort based on start of the interval
        Arrays.sort(intervals, (o1, o2) -> (o1[0] > o2[0]?1:( o1[0]<o2[0]?-1:0 )));

        int previousEnd=intervals[0][1];

        int count = 0;

        for(int i=1; i<intervals.length; i++){

            //overlapping
            if(intervals[i][0] < previousEnd){

                //we choose to keep the interval whose end is smaller because there is less chance that it will conflict later
                //increase the count, as we need to remove one of the intervals
                count++;
                previousEnd = Math.min(previousEnd, intervals[i][1]);

            }else{

                //update end depending on which is max (current or previous)
                previousEnd = Math.max(previousEnd, intervals[i][1]);
            }

        }

        return count;

    }

}
```
