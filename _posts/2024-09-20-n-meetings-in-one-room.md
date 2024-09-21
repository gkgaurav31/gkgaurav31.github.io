---
layout: post
title: N meetings in one room (geeksforgeeks - SDE Sheet)
date: 2024-09-20 23:49 +0530
author: "Gaurav Kumar"
tags: "java intervals geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

You are given timings of n meetings in the form of (start[i], end[i]) where start[i] is the start time of meeting i and end[i] is the finish time of meeting i. Return the maximum number of meetings that can be accommodated in a single meeting room, when only one meeting can be held in the meeting room at a particular time.

Note: The start time of one chosen meeting can't be equal to the end time of the other chosen meeting.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/maximum-path-sum/1?page=8)

## SOLUTION

We can solve this by using a greedy algorithm where we first pair the start and end times of each meeting, then sort all meetings based on their end times in ascending order. This allows us to always select the meeting that finishes the earliest, maximizing the number of meetings we can accommodate. After sorting, we iterate through the meetings, checking if each meeting starts after the previous one finishes. If it does, we include that meeting and update the end time to the current meeting's end time.

```java
class Solution {

    public int maxMeetings(int n, int start[], int end[]) {

        // Create a 2D array to store the start and end times of each meeting
        int[][] meetings = new int[n][2];

        for (int i = 0; i < n; i++) {
            meetings[i] = new int[]{start[i], end[i]};
        }

        // Sort the meetings based on their end time (ascending order)
        Arrays.sort(meetings, (a, b) -> a[1] - b[1]);

        // We can always attend at least one meeting (first one)
        int possible = 1;

        // Initialize prevEnd to store the end time of the last selected meeting
        int prevEnd = meetings[0][1];

        for (int i = 1; i < n; i++) {

            // Check if the start time of the current meeting is after the last selected meeting's end time
            if (meetings[i][0] > prevEnd) {

                // This meeting can be attended, so increment the count
                possible++;

                // Update prevEnd to the current meeting's end time
                prevEnd = meetings[i][1];

            }

        }

        return possible;
    }

}
```
