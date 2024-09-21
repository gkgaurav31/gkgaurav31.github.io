---
layout: post
title: Minimum number of Coins (geeksforgeeks - SDE Sheet)
date: 2024-09-21 13:19 +0530
author: "Gaurav Kumar"
tags: "java strings greedy geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given an infinite supply of each denomination of Indian currency { 1, 2, 5, 10, 20, 50, 100, 200, 500, 2000 } and a target value N.
Find the minimum number of coins and/or notes needed to make the change for Rs N. You must return the list containing the value of coins required.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/-minimum-number-of-coins4426/1?page=8)

## SOLUTION

We can solve this by using a greedy approach. The idea is to use the largest possible coin first to reduce the amount N as quickly as possible. We have an array of coin denominations sorted in descending order. Starting from the largest coin, we repeatedly subtract the coin's value from N and add that coin to the result list. If a coin is too large to fit into N, we move to the next smaller denomination. This process continues until N becomes zero, and the list of coins used to make the amount is returned.

```java
class Solution {

    static List<Integer> minPartition(int N) {

        int[] coins = {2000, 500, 200, 100, 50, 20, 10, 5, 2, 1};

        // List to store the selected coins
        List<Integer> list = new ArrayList<>();

        // Index to track which denomination we're currently considering
        int idx = 0;

        // While we haven't finished breaking down the amount N and haven't exhausted all denominations
        while (N > 0 && idx < coins.length) {

            // If the current denomination can fit into the remaining amount N
            if (coins[idx] <= N) {

                // Add the current denomination to the list
                list.add(coins[idx]);

                // Subtract the value of the current coin from N
                N -= coins[idx];

            } else {

                // Move to the next smaller denomination
                idx++;

            }
        }

        return list;
    }

}
```
