---
layout: post
title: Minimum Platforms (geeksforgeeks - SDE Sheet)
date: 2024-08-24 19:09 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks arrays sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given arrival and departure times of all trains that reach a railway station. Find the minimum number of platforms required for the railway station so that no train is kept waiting.
Consider that all the trains arrive on the same day and leave on the same day. Arrival and departure time can never be the same for a train but we can have arrival time of one train equal to departure time of the other. At any given instance of time, same platform can not be used for both departure of a train and arrival of another train. In such cases, we need different platforms.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/minimum-platforms-1587115620/1?page=1)

## SOLUTION

```java
class Solution
{
    //Function to find the minimum number of platforms required at the
    //railway station such that no train waits.
    static int findPlatform(int arr[], int dep[], int n)
    {

        if(n == 0)
            return 0;

        // sort arrival so that we can first check if any platform can be made available for the earliest required train
        Arrays.sort(arr);

        // we will sort the departure times also
        // init: the first endtime will be added to minheap for comparison with rest of the trains
        Arrays.sort(dep);

        PriorityQueue<Integer> pq = new PriorityQueue<>();
        pq.add(dep[0]); // first/earliest endTime of the train

        // at least one platform is needed for the first train
        int platforms = 1;

        // check other trains
        for(int i=1; i<n; i++){

            // current start the train
            int currentArr = arr[i];

            // If the earliest departing train's departure time is before the current train's arrival time,
            // that platform can be freed up. Remove that departure time from the priority queue.
            if(pq.peek() < currentArr){
                pq.poll();

            // If no platform is free, increase the platform count.
            }else{
                platforms++;
            }

            // add the departure time of current train
            pq.add(dep[i]);

        }

        return platforms;

    }

}
```
