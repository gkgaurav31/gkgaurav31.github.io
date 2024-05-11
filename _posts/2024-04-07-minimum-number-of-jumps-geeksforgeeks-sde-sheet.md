---
layout: post
title: Minimum number of jumps (geeksforgeeks - SDE Sheet)
date: 2024-04-07 12:53 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks arrays sde-sheet important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given an array of `N` integers `arr[]` where each element represents the maximum length of the jump that can be made forward from that element. This means if `arr[i] = x`, then we can jump any distance `y` such that `y ≤ x`.
Find the minimum number of jumps to reach the end of the array (starting from the first element). If an element is `0`, then you cannot move through that element.

Note: Return `-1`if you can't reach the end of the array.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/minimum-number-of-jumps-1587115620/1?page=1)

## SOLUTION

### APPROACH 1 (TLE)

For each position, check all its previous positions and try to figure out the minimum possible steps. If the previous step needed x steps, and it is possible to reach from previous step to current step, then the number of steps will be x+1. Keep doing this for all previous position which can be used to reach the current step and choose the minimum amongst them all.

```java
class Solution{

    static int minJumps(int[] arr){

        int n = arr.length;

        int[] dp = new int[n];

        // init to max value since we need to figure out min later
        Arrays.fill(dp, Integer.MAX_VALUE);

        // 0 steps needed for index 0
        dp[0] = 0;

        for(int i=1; i<n; i++){

            for(int j=0; j<i; j++){

                // it's not possible to reach j, so there's no point in checking if it's possible to reach i from j
                if(dp[j] == Integer.MAX_VALUE)
                    continue;

                // if it's possible reach j from i
                if(arr[j] + j >= i)

                    // choose min between (previous number of steps possible from other options, min steps needed to reach j plus 1)
                    dp[i] = Math.min(dp[i], dp[j] + 1);

            }

        }

        // if it's not possible to reach the end, the value will remain INT_MAX
        return dp[n-1]==Integer.MAX_VALUE?-1:dp[n-1];

    }

}
```

### APPROACH 2

We begin from the end of the array and iterate backwards. We maintain the current position and tally the number of jumps needed. At each step, we identify the leftmost position from which we can jump to the current position. If it's not feasible to reach the current position from any previous one, we return -1 to denote impossibility. Otherwise, we return the total count of jumps required to reach the array's end.

```java
class Solution{

    static int minJumps(int[] arr){

        int n = arr.length;

        // current position to reach
        int current = n-1;

        // number of jumps
        int jumps = 0;

        // we start from right, and check the left most position from which we can jump to this position
        while(current > 0){

            // since we are still not at index 0, we need 1 more jump
            jumps++;

            // init: try to check the left most position before current from which we can reach current position
            int next = current;

            // check for all positions before current
            // if it's possible to reach current position update it
            // get the minimum index (left most index) from which we can reach current
            for(int i=current-1; i>=0; i--){
                if(i+arr[i] >= current){
                    next = i;
                }
            }

            // this means, we cannot reach the current position from any previous position
            // so, it's not possible to reach the end at all; return -1
            if(current == next)
                return -1;

            // otherwise, it's possible to reach current position
            // so now we can update our current goal to the left most position from which we can reach current
            // this will be our next goal
            current = next;

        }

        return jumps;


    }

}
```
