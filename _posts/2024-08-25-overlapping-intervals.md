---
layout: post
title: Overlapping Intervals (geeksforgeeks - SDE Sheet)
date: 2024-08-25 17:55 +0530
author: "Gaurav Kumar"
tags: "java arrays intervals geeksforgeeks"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a collection of Intervals, the task is to merge all of the overlapping Intervals.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/overlapping-intervals--170633/1?page=2)

## SOLUTION

```java
class Solution
{
    // Method to merge all overlapping intervals
    public int[][] overlappedInterval(int[][] intervals)
    {
        int n = intervals.length;

        // Sort intervals based on the starting point, and if they have the same start, sort by the ending point
        Arrays.sort(intervals, (a, b) -> a[0] == b[0] ? a[1] - b[1] : a[0] - b[0]);

        // List to hold merged intervals
        List<int[]> list = new ArrayList<>();
        list.add(intervals[0]); // Add the first interval to the list

        // The previous interval for comparison
        // We could have also used the list directly
        int[] prevInterval = intervals[0];

        // Iterate through all intervals
        for(int i = 1; i < n; i++){

            // Get the current interval
            int[] current = intervals[i];

            // If current interval does not overlap with the previous interval
            if(current[0] > prevInterval[1]){

                list.add(current); // Add the current interval to the list
                prevInterval = current; // Update previous interval

            } else {

                // Merge the current interval with the previous interval
                prevInterval[1] = Math.max(prevInterval[1], current[1]);

            }
        }

        // Convert the list of merged intervals to a 2D array
        int[][] res = new int[list.size()][2];
        for(int i = 0; i < list.size(); i++)
            res[i] = list.get(i);

        return res;
    }
}
```
