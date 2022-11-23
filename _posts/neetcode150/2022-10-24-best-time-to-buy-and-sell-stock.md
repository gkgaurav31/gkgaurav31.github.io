---
layout: post
title: Best Time to Buy and Sell Stock
date: 2022-10-24 00:04 +0530
author: "Gaurav Kumar"
tags: "java neetcode150"
categories: "neetcode150"
---

This question is part of [NeetCode150 series](https://neetcode.io/practice).  

## Problem Description

You are given an array prices where prices[i] is the price of a given stock on the ith day.

You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock.

Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return 0.

[leetcode](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

### Solution

### APPROACH 1

For each index, if we knnow the largest number on its right, we can consider that as a possible solution. To optimize it, we can create a prefix/suffix array which will give us a largest element on the right side for each i. So, all we need to do is iterate through our actual list, calculate the profit and check if it's more than the current max.

```java
class Solution {
    
    public int maxProfit(int[] prices) {
     
        int n = prices.length;
        
        int[] right = new int[n];
        
        right[n-1] = 0;
        for(int i=n-2; i>=0; i--){
            right[i] = Math.max(right[i+1], prices[i+1]);
        }
        
        int maxProfit = 0;
        
        for(int i=0; i<n-1; i++){
            int buyPrice = prices[i];
            int sellPrice = right[i];
            maxProfit = Math.max(maxProfit, sellPrice-buyPrice);
        }
        
        return maxProfit;
        
    }
    
}
```

### APPROACH 2

We can further optimize it without using any extra space. The idea is the keep a track of minimum value as we iterate through the array. Whenever we get a smaller element, we update  the min. We get the current profit by comparing the current element with this minimum number we would have got earlier. If it's more than the max profit, we update it.

```java
class Solution {
    
    public int maxProfit(int[] prices) {
    
        int n = prices.length;
        
        int currentMin = Integer.MAX_VALUE;
        int maxProfit = 0;
        
        //Loop till n-2 because we cannot sell the last stock
        for(int i=0; i<n; i++){
            
            currentMin = Math.min(currentMin, prices[i]);
            int profit = prices[i] - currentMin;    //As per the question, we cannot sell on the same day. However, in this case the profit will be 0, so it does not matter using this logic.
            maxProfit = Math.max(profit, maxProfit);
            
        }
        
        return maxProfit;
    
    }
    
}
```
