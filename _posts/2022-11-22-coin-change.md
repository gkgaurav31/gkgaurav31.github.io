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

    private static final int MAX = 10001; //constraint: 0 <= amount <= 10^4

    public int coinChange(int[] coins, int amount) {

        //dp[i] denotes the minimum number of coins needed to make an amount "i" using the coins given
        int[] dp = new int[amount+1];

        //if the maximum aount is 100, the maximum possible answer can be 100 using all 1s. So, we init the dp array to something more than that which will mean that it's not possible to make up that amount. We don't set it to -1 because we will try to calculate the minimum number of coins later, so -1 would never get updated.
        Arrays.fill(dp, MAX);

        //0 coins needed to make an amount of 0
        dp[0] = 0;

        //calculate the number of coins needed to make an amount from 1, 2, 3...,amount needed
        for(int i=1; i<=amount; i++){

            //try for each coin available and make use of pre-calculated values in the dp array to find the minimum amoung all possible options
            for(int j=0; j<coins.length; j++){

                //the current amount must be >= current coin value, otherwise the amount will be negative (not possible)
                if(i>=coins[j]){
                    //let's say that the current amount is x
                    //let's say the current coin is y
                    //x-y>=0, so it would be possible to use the current coin
                    //if we use the current coin, we will need to add 1 extra coin used
                    //how many coins are needed for the remaining sum which is x-y?
                    //this will be pre-calculated in dp at dp[x-y]!
                    //this is one possible answer, but may not be the minimum
                    //so update it only if it's lower than the previous options (using other coins)
                    dp[i] = Math.min(dp[i], 1 + dp[i-coins[j]]);
                }

            }

        }

        return dp[amount]==MAX?-1:dp[amount];

    }

}
```
