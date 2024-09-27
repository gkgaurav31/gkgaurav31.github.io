---
layout: post
title: Egg Dropping Puzzle (geeksforgeeks - SDE Sheet)
date: 2024-09-26 22:29 +0530
author: "Gaurav Kumar"
tags: "java dynamic_programming geeksforgeeks sde-sheet important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

You are given N identical eggs and you have access to a K-floored building from 1 to K.

There exists a floor f where 0 <= f <= K such that any egg dropped from a floor higher than f will break, and any egg dropped from or below floor f will not break.
There are few rules given below.

An egg that survives a fall can be used again.
A broken egg must be discarded.
The effect of a fall is the same for all eggs.
If the egg doesn't break at a certain floor, it will not break at any floor below.
If the eggs breaks at a certain floor, it will break at any floor above.

Return the minimum number of moves that you need to determine with certainty what the value of f is.

For more description on this problem see wiki page

[geeksforgeeks](https://www.geeksforgeeks.org/problems/egg-dropping-puzzle-1587115620/1?page=9)

## SOLUTION

### BRUTE-FORCE (TLE)

1. Base Cases:

   - If there is only 1 egg, the minimum number of attempts is equal to the number of floors (`floors`).
   - If there are 0 or 1 floors, return `floors` as no attempts are needed.

2. Recursive Function:

   - For each floor `i` (from `1` to `floors`):
     - Egg breaks: Check floors below (`min_moves(eggs - 1, i - 1)`).
     - Egg does not break: Check floors above (`min_moves(eggs, floors - i)`).
   - Take the maximum of the above two scenarios to cover the worst case.
   - Update the minimum attempts (`ans`) as `1 + max(op1, op2)`.

3. Return Result:
   - The function returns the minimum number of attempts needed (`ans`) after considering all possible floors.

```java
class Solution
{
    static int eggDrop(int n, int k)
    {
        return eggDropHelper(n, k);
    }

    static int ans = Integer.MAX_VALUE;

    static int eggDropHelper(int eggs, int floors){

        // Base case 1: If only one egg is left, we need to try every floor sequentially.
        if(eggs == 1)
            return floors;

        // Base case 2: If there are no floors or only one floor, return the number of floors.
        if(floors == 0 || floors == 1)
            return floors;

        // Minimum moves for the current state.
        int moves = Integer.MAX_VALUE;

        // Try dropping an egg from each floor (1 to 'floors') and calculate the worst-case scenario.
        for(int i = 1; i <= floors; i++){

            // Case 1: Egg breaks, so we only need to check the floors below 'i' with one less egg.
            int o1 = eggDropHelper(eggs - 1, i - 1);

            // Case 2: Egg does not break, so we check the floors above 'i' with the same number of eggs.
            int o2 = eggDropHelper(eggs, floors - i);

            // Calculate the worst-case scenario for current floor 'i'.
            int temp = 1 + Math.max(o1, o2);

            // Update 'moves' with the minimum attempts required for the worst case.
            moves = Math.min(moves, temp);

        }

        return moves;

    }
}
```

### DYNAMIC PROGRAMMING

```java
class Solution
{

    static int eggDrop(int n, int k)
    {
        // dp array to store the minimum attempts needed
        // for given number of eggs and floors
        int[][] dp = new int[n + 1][k + 1];

        for(int i = 0; i <= n; i++){
            Arrays.fill(dp[i], -1);
        }

        eggDropHelper(n, k, dp);

        return dp[n][k];
    }

    static int eggDropHelper(int eggs, int floors, int[][] dp){

        // Base case: If we have only 1 egg, the worst case scenario is to try all floors one by one, so the result will be the number of floors.
        if(eggs == 0 || eggs == 1)
            return dp[eggs][floors] = floors;

        // Base case: If there are no floors, no trials are needed.
        if(floors == 0)
            return dp[eggs][floors] = 0;

        if(dp[eggs][floors] == -1){

            // Initialize result as a large value.
            // This would also work: int res = floors;
            int res = Integer.MAX_VALUE;

            // Try dropping the egg from each floor 1 to `floors` and take the minimum of all possible maximums of these attempts.
            for(int k = 1; k <= floors; k++){

                // If the egg breaks, check floors below the current one (k-1) with one less egg.
                // If the egg doesn't break, check floors above the current one (floors-k) with the same number of eggs.
                // Take max to cover the worst case
                int current = 1 + Math.max(eggDropHelper(eggs - 1, k - 1, dp),
                                           eggDropHelper(eggs, floors - k, dp));

                // Choose the minimum attempts needed among all attempts.
                res = Math.min(res, current);
            }

            dp[eggs][floors] = res;

        }

        return dp[eggs][floors];
    }
}
```
