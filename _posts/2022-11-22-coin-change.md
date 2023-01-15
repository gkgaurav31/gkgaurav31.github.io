---
layout: post
title: Coin Change
date: 2022-11-22 23:37 +0530
author: "Gaurav Kumar"
tags: "java leetcode dynamic_programming neetcode150 important"
categories: "neetcode150"
---

## Problem Description

You are given an integer array coins representing coins of different denominations and an integer amount representing a total amount of money.

Return the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.

You may assume that you have an infinite number of each kind of coin.

### Solution

[NeetCode YouTube](https://www.youtube.com/watch?v=H9bfqozjoqs)

### BRUTE FORCE

```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        return coinHelper(coins, amount);   
    }

    public int min = Integer.MAX_VALUE;

    public int coinHelper(int[] coins, int amount){

        //0 coins needed for amount 0
        if(amount == 0) return 0;

        //if amount is -ve, it's not possible
        if(amount < 0) return -1;

        int c = Integer.MAX_VALUE;

        //loop through each coin
        for(int i=0; i<coins.length; i++){

            //get count of coins for amount-coins[i]
            int count = coinHelper(coins, amount - coins[i]);

            //if amount-coins[i] is not possible, skip
            if(count == -1) continue;

            //update min value
            c = Math.min(c, count + 1); //added 1 because we used one coin by subtracting from amount
        }

        //if c is still Integer.MAX_VALUE, we did not find any possible combination of coins
        return (c == Integer.MAX_VALUE?-1:c);

    }
}
```

### BOTTOM-UP APPROACH - DYNAMIC PROGRAMMING

```java
class Solution {

    private static final int MAX = 10001; //constraint: 0 <= amount <= 104
    
    public int coinChange(int[] coins, int amount) {

        int[] dp = new int[amount+1];
        Arrays.fill(dp, MAX);
        
        dp[0] = 0;

        for(int i=1; i<=amount; i++){

            for(int j=0; j<coins.length; j++){
                if(i>=coins[j]){
                    dp[i] = Math.min(dp[i], 1 + dp[i-coins[j]]);
                }
            }

        }

        return dp[amount]==MAX?-1:dp[amount];
        
    }

}
```
