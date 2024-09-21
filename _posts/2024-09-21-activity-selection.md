---
layout: post
title: Activity Selection (geeksforgeeks - SDE Sheet)
date: 2024-09-21 17:22 +0530
author: "Gaurav Kumar"
tags: "java greedy geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given N activities with their start and finish day given in array start[ ] and end[ ]. Select the maximum number of activities that can be performed by a single person, assuming that a person can only work on a single activity at a given day.
Note : Duration of the activity includes both starting and ending day.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/activity-selection-1587115620/1?page=8)

## SOLUTION

This is similar to: [N Meetings in One Room]({% post_url 2024-09-20-n-meetings-in-one-room %})

```java
class Solution
{

    public static int activitySelection(int start[], int end[], int n)
    {
        // 2D array to store start and end times of activities
        int[][] activity = new int[n][2];

        for(int i = 0; i < n; i++){
            activity[i] = new int[]{start[i], end[i]};
        }

        // Sort activities based on their end times
        Arrays.sort(activity, (a, b) -> a[1] - b[1]);

        // 1 activity can be done always (n > 0)
        int count = 1;

        // End time of the first activity
        int prevEnd = activity[0][1];

        // Iterate through other activities
        for(int i = 1; i < n; i++){

            // If the start time of the current activity is greater than the end time
            // of the last selected activity, we can select it
            if(activity[i][0] > prevEnd){

                count++;

                // Update the end time to the current activity's end time
                prevEnd = activity[i][1];

            }
        }

        return count;
    }
}
```
