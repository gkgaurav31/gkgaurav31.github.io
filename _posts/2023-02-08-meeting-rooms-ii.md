---
layout: post
title: Meeting Rooms II
date: 2023-02-08 23:10 +0530
author: "Gaurav Kumar"
tags: "java leetcode intervals neetcode150 important"
categories: "neetcode150"
---

## PROBLEM DESCRIPTION

Given an array of meeting time intervals intervals where intervals[i] = [starti, endi], return the minimum number of conference rooms required.

[leetcode](https://leetcode.com/problems/meeting-rooms-ii/description/)

### SOLUTION

### APPROACH 1

```java
class Solution {
    
    public int minMeetingRooms(int[][] intervals) {

        //sort by start time of the meetings
        Arrays.sort(intervals, (o1, o2) -> o1[0] > o2[0]?1: (o1[0] < o2[0]?-1:0) );

        //min heap -> to store the end times of the meetings
        //we need to this find if there is any meeting which will end before the current meeting starts
        //if the meeting which has the least end time does not end, then no other meeting will also end
        //this is why we use a min heap
        PriorityQueue<Integer> endTimes = new PriorityQueue<>();
        endTimes.add(intervals[0][1]);

        //track number of rooms needed
        //we increasement by 1 when there are no rooms available
        int rooms = 1;

        //check other meetings
        for(int i=1; i<intervals.length; i++){

            //if current meeting starts after any of the previous meetings have ended
            //we only need to check the min end time amongst all of them, for which we have created min heap
            //one of the previous room can be re-used
            if(intervals[i][0] >= endTimes.peek()){

                //since we will using that meeting room, remove it from min heap
                endTimes.poll();

                //add the end time of current interval
                endTimes.add(intervals[i][1]);

            //no rooms are free
            }else{

                //we need to allocate new room, so increment by 1
                rooms++;

                //add the end time of current meeting
                endTimes.add(intervals[i][1]);
                
            }

        }

        return rooms;

    }

}
```

### APPROACH 2

[NeetCode YouTube](https://www.youtube.com/watch?v=FdzJmTCVyJU)

```java
class Solution {

    public int minMeetingRooms(int[][] intervals) {

        //create list of start and end timings, both sorted in ascending order
        List<Integer> startTimings = new ArrayList<>();
        List<Integer> endTimings = new ArrayList<>();
        
        for(int i=0; i<intervals.length; i++){
            startTimings.add(intervals[i][0]);
            endTimings.add(intervals[i][1]);
        }

        Collections.sort(startTimings);
        Collections.sort(endTimings);
        
        //current number of rooms
        int rooms = 0;

        //track max rooms
        int maxRooms=1;

        //i -> start timings
        //j -> end timings
        int i=0, j=0;

        //check all meetings using start timing
        while(i<startTimings.size()){
            
            //if meeting needs to start before current end
            if(startTimings.get(i) < endTimings.get(j)){

                //we need +1 room
                rooms++;

                //move to next start
                i++;
            
            //meeting has ended, so reduce rooms by 1
            }else{
                rooms--;
                j++;
            }

            //update max
            maxRooms = Math.max(maxRooms, rooms);

        }

        return maxRooms;

    }
    
}
```
