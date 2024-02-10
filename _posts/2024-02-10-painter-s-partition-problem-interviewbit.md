---
layout: post
title: Painter's Partition Problem (InterviewBit)
date: 2024-02-10 12:22 +0530
author: "Gaurav Kumar"
tags: "java binary_search"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given 2 integers `A` and `B` and an array of integers `C` of size `N`. Element `C[i]` represents the length of ith board.
You have to paint all `N` boards `[C0, C1, C2, C3 … CN-1]`. There are `A` painters available and each of them takes `B` units of time to paint 1 unit of the board.

Calculate and return the minimum time required to paint all boards under the constraints that any painter will only paint contiguous sections of the board.

NOTE:

1. 2 painters cannot share a board to paint. That is to say, a board cannot be painted partially by one painter, and partially by another.
2. A painter will only paint contiguous boards. This means a configuration where painter 1 paints boards 1 and 3 but not 2 is invalid.

Return the `ans % 10000003`.

## SOLUTION

We can use binary search to solve this problem. The range of possible answer can be `[1, total time needed if there was only 1 worker]`. As we do in binary search, pick up the middle value and try to check if that is possible. Let us say we picked mid `m`. So, the next question is "Is it possible to paint all boards using `A` painters within `m` time?". This is actually the trickier part of the problem where we need to use a greedy approach. Note that the problem mentions that all the painters need to take contiguous boards only and all of them take `B` unit time to paint 1 unit of length. So, "it does not matter which painter will paint a given board" since all of them are equivalent. Hence, we can solve this problem in a greedy way by trying to allocate the maximum number of boards to any painter.

We can start by iterating from left to right on the boards. If it's not even possible to paint that board since it can take more than `m` time, we can simply return false.  
If it's within that limit, we should check if can be assigned to the current painter itself. If so, do it. If not, increase the number of painters that we need. Also, add this extra time needed to paint the current board. Keep doing this for all the boards. At the end, we will have the minimum number of painters needed to paint all boards. If it's <= total number of painters `B`, we can return true.

Once we know that it's possible to paint all boards in `m` time, that is a possible answer. Save it and try to look for even lesser value. If it's not possible, it wouldn't be possible it we had lesser time. So look for higher time values.

```java
public class Solution {

    final long M = 10000003; // Modulus value

    public int paint(int A, int B, int[] C) {

        int n = C.length; // Number of boards

        // binary search range:
        // at least 1 unit of time is needed (it may not be possible to complete all though which we will check later)
        long l=1;
        // maximum possible time needed (assume there is only 1 worker)
        long r=maxPossibleTime(C, B);

        // init: minimum time needed to paint all boards
        long minTimeNeeded = r;

        // Binary search to find the minimum time needed
        while(l<=r){

            long m = (l+r)/2; // Midpoint

            // Checking if it's possible to paint with current time limit
            if(isPossible(A, B, C, m)){

                // since it's possible, record this answer and try to minize this
                minTimeNeeded = m%M;

                // go to left since we want to minimize the time
                r = m-1;

            }else{

                // Since it's not possible with current time limit, try with larger value. So go to right side
                l = m+1;
            }

        }

        return (int) (minTimeNeeded%M);

    }

    // Function to check if it's possible to paint with given constraints and time limit
    public boolean isPossible(long totalPainters, long timeNeededPerUnitOfBoard, int[] boards, long maxTimeAllowed){

        int n = boards.length;

        long currentTotalTime = 0;
        long currentTotalPainters = 0;

        // Iterate through all boards and greedily try to add more board for the current painter. If it's not possible to add, then only use another painter
        // It may happen that the current board cannot be completed at all in the given time limit, in which we can directly return false
        for(int i=0; i<n; i++){

            // edge case: it's not possible for the current work to be done at all within maxTimeAllowed limit
            if(boards[i] * timeNeededPerUnitOfBoard > maxTimeAllowed)
                return false;

            // If the current board can be complete within the time limit by the current painter, do it.
            if(boards[i] * timeNeededPerUnitOfBoard + currentTotalTime <= maxTimeAllowed){

                // update the total time which the current painter will spend
                currentTotalTime += boards[i] * timeNeededPerUnitOfBoard;

            // If the next board cannot be taken up by the current painter, we will need another painter to be added
            }else{

                // Increment total painters used
                currentTotalPainters++;

                // Reset total time
                currentTotalTime = boards[i] * timeNeededPerUnitOfBoard;
            }

        }

        // Increment total painters if there's remaining time
        if(currentTotalTime > 0){
            currentTotalPainters++;
        }

        // Check if total painters used is within limit
        return currentTotalPainters <= totalPainters;

    }

    public long maxPossibleTime(int[] boards, long timePerUnit){
        long totalLength = 0;
        for(int i=0; i<boards.length; i++) totalLength += boards[i];
        return totalLength * timePerUnit;
    }


}
```
