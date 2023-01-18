---
layout: post
title: Best Time to Buy and Sell Stock with Cooldown
date: 2023-01-18 23:28 +0530
author: "Gaurav Kumar"
tags: "java leetcode dynamic_programming neetcode150 important"
categories: "neetcode150"
---

## PROBLEM DESCRIPTION

You are given an array prices where prices[i] is the price of a given stock on the ith day.
Find the maximum profit you can achieve. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times) with the following restrictions:

- After you sell your stock, you cannot buy stock on the next day (i.e., cooldown one day).

Note: You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

[leetcode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/description/)

### SOLUTION

[NeetCode YouTube](https://www.youtube.com/watch?v=I7j0F7AHpb8)

```java
class Solution {

    public int maxProfit(int[] prices) {
        return profitHelper(prices, true, 0, new HashMap<>());
    }

    public int profitHelper(int[] prices, boolean buying, int idx, Map<String, Integer> map){

        if(idx >= prices.length) return 0;

        if(!map.containsKey(idx+":"+buying)){

            if(buying){

                int amount = profitHelper(prices, !buying, idx+1, map) - prices[idx];
                int cooldown = profitHelper(prices, buying, idx+1, map);

                int max = Math.max(amount, cooldown);

                map.put(idx+":"+buying, max);

            }else{

                int amount = profitHelper(prices, !buying, idx+2, map) + prices[idx];
                int cooldown = profitHelper(prices, buying, idx+1, map);

                int max = Math.max(amount, cooldown);

                map.put(idx+":"+buying, max);

            }

        }

        return map.get(idx+":"+buying);

    }
}
```
