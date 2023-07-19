---
layout: post
title: Best Time to Buy and Sell Stock II
date: 2023-07-19 22:57 +0530
author: "Gaurav Kumar"
tags: "java arrays leetcode leetcode150 important"
categories: "arrays"
---

## PROBLEM DESCRIPTION

You are given an integer array prices where prices[i] is the price of a given stock on the ith day.

On each day, you may decide to buy and/or sell the stock. You can only hold at most one share of the stock at any time. However, you can buy it then immediately sell it on the same day.

Find and return the maximum profit you can achieve.

[leetcode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)

## SOLUTION

![snapshot]({{ site.baseurl }}/assets/img/buy_sell_stock_2.png)

```java
class Solution {

    public int maxProfit(int[] prices) {
        
        int total=0;

        for(int i=1; i<prices.length; i++){

            //whenever there is an increase in price, take the profit
            //this will be equal to, if you have bought at the lowest (valley) and sold at the next heighest price (peak)
            if(prices[i] > prices[i-1]){
                total += (prices[i] - prices[i-1]);
            }
        }

        return total;

    }
    
}
```
