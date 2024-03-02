---
layout: post
title: Insert Interval
date: 2023-02-07 21:41 +0530
author: "Gaurav Kumar"
tags: "java leetcode intervals neetcode150 important"
categories: "neetcode150"
---

## PROBLEM DESCRIPTION

You are given an array of non-overlapping intervals intervals where intervals[i] = [starti, endi] represent the start and the end of the ith interval and intervals is sorted in ascending order by starti. You are also given an interval newInterval = [start, end] that represents the start and end of another interval.

Insert newInterval into intervals such that intervals is still sorted in ascending order by starti and intervals still does not have any overlapping intervals (merge overlapping intervals if necessary).

Return intervals after the insertion.

[leetcode](https://leetcode.com/problems/insert-interval/description/)

### SOLUTION

[NeetCode YouTube](https://www.youtube.com/watch?v=A8NUOmlwOlM)

```java
class Solution {

    public int[][] insert(int[][] intervals, int[] newInterval) {

        List<int[]> list = new ArrayList<>();

        //Loop through each interval
        for(int i=0; i<intervals.length; i++){

            //newInterval is before the currentInterval (no overlap)
            if(newInterval[1] < intervals[i][0]){

                //add newInterval first to the answer list
                list.add(newInterval);

                //all other intervals will come after it
                for(int j=i; j<intervals.length; j++){
                    list.add(intervals[j]);
                }

                //this is also our answer, so return it
                return convert(list);

            //if newInterval is after currentInterval (no overlap)
            }else if(newInterval[0] > intervals[i][1]){

                //add the current interval
                //but continue with next intervals because they can still have overlap
                list.add(intervals[i]);

            //if newInterval is neighter before nor after current interval, it must be overlapping
            }else{

                //merge the newInterval
                //after merge, the newInterval's start will be min(start of newInterval, start of currentInterval)
                //after merge, the newInterval's end will be max(end of newInterval, end of currentInterval)
                newInterval[0] = Math.min(newInterval[0], intervals[i][0]);
                newInterval[1] = Math.max(newInterval[1], intervals[i][1]);

            }

        }

        //If we reach here, we have either added the previous intervals or merged them
        //But while doing so, we never add the newInterval formed to the answer list (because they could merge more)
        //So, now we add the newInterval to the answer list
        list.add(newInterval);

        // we can also use an in-built method for conversion
        // return list.toArray(new int[list.size()][]);
        return convert(list);

    }

    public int[][] convert(List<int[]> list){
        int[][] res = new int[list.size()][2];
        for(int i=0; i<list.size(); i++) res[i] = list.get(i);
        return res;
    }

}
```
