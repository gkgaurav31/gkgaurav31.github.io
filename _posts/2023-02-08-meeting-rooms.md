---
layout: post
title: Meeting Rooms
date: 2023-02-08 00:00 +0530
author: "Gaurav Kumar"
tags: "java leetcode intervals neetcode150"
categories: "neetcode150"
---

## PROBLEM DESCRIPTION

Given an array of meeting time intervals where intervals[i] = [starti, endi], determine if a person could attend all meetings.

[leetcode](https://leetcode.com/problems/meeting-rooms/description/)

## SOLUTION

```java
class Solution {

    public boolean canAttendMeetings(int[][] intervals) {

        //no meeting
        if(intervals.length == 0) return true;

        //sort based on start time
        Arrays.sort(intervals, (o1, o2) -> o1[0] > o2[0]?1: (o1[0] < o2[0]?-1:0) );

        //save previous meetine end time
        int previousEnd = intervals[0][1];

        //check other meetings in order
        for(int i=1; i<intervals.length; i++){

            //if next meeting starts before previous meeting end, return false
            if(intervals[i][0] < previousEnd) return false;

            //otherwise, update previousEnd as per the end time of next meeting
            previousEnd = intervals[i][1];

        }

        //no conflict found
        return true;

    }

}
```

### ANOTHER WAY TO CODE

```java
class Solution {

    public boolean canAttendMeetings(int[][] intervals) {

        // number of intervals
        int n = intervals.length;

        // sort based on start time of the intervals
        Arrays.sort(intervals, (a,b) -> a[0] - b[0] );

        // go to each intervals
        // since we have already sorted the intervals based on start time, we know that the second meeting will start later
        // the conflict will happen if the previous meeting has not ended
        for(int i=1; i<n; i++){

            // if previous meeting is still going on and it is overlapping with next interval, return false
            if(intervals[i][0] < intervals[i-1][1]) return false;

        }

        return true;

    }

}
```
