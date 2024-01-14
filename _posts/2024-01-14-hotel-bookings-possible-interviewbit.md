---
layout: post
title: Hotel Bookings Possible (InterviewBit)
date: 2024-01-14 23:06 +0530
author: "Gaurav Kumar"
tags: "java interviewbit arrays"
categories: "interviewbit"
---

## Problem Description

A hotel manager has to process N advance bookings of rooms for the next season. His hotel has C rooms. Bookings contain an arrival date and a departure date. He wants to find out whether there are enough rooms in the hotel to satisfy the demand. Write a program that solves this problem in time O(N log N) .
Note- If we have arrival and departure on the same date then arrival must be served before the departure.

[InterviewBit](https://www.interviewbit.com/problems/hotel-bookings-possible/)

### Solution

This problem can we solved in the same way as [Meeting Rooms II]({% post_url 2023-02-08-meeting-rooms-ii %}).

```java
public class Solution {

    public boolean hotel(ArrayList<Integer> arrive, ArrayList<Integer> depart, int K) {

        int n = arrive.size();

        // Sort arrival and departure lists separately
        Collections.sort(arrive);
        Collections.sort(depart);

        int rooms = 0;

        int i = 0; // Pointer for arrival list
        int j = 0; // Pointer for departure list

        while (i < n) {

            // If the current arrival is before or equal to the current departure
            // That means, we will need one more room
            if (arrive.get(i) <= depart.get(j)) {

                rooms++; // Increment rooms as a new guest arrives
                i++;     // Move to the next arrival

            // Otherwise, there is one meeting which ended, we one room is free
            } else {

                j++;     // Move to the next departure, a guest has left
                rooms--; // Decrement rooms as a guest departs

            }

            // Check if the number of rooms in use exceeds the available rooms
            if (rooms > K)
                return false; // Not enough rooms available

        }

        // If the loop completes without returning false, there are enough rooms
        return true;

    }
}
```
