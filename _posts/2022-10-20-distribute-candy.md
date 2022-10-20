---
layout: post
title: Distribute Candy
date: 2022-10-20 22:51 +0530
author: "Gaurav Kumar"
tags: "java heap greedy leetcode important"
categories: "greedy"
---

## Problem Description

There are n children standing in a line. Each child is assigned a rating value given in the integer array ratings.  

You are giving candies to these children subjected to the following requirements:  

- Each child must have at least one candy.
- Children with a higher rating get more candies than their neighbors.

Return the minimum number of candies you need to have to distribute the candies to the children.

### Solution

Create two arrays of size N - left and right. In the left array: let's say that the first person get 1 candy. We will go towards right and check its left element. If the current element has more value than its left, we will give it one more candy.  

In the same way, we will populate the "right" array. Set 1 candy for the last person. Go towards left. If the current person has more value than its right, we give is one extra candy.

We need to somehow make use of these two data points in left and right arrays. If we carefully observe, the final results needs to be Math.max(left[i], right[i]).

```java
class Solution {

    public int candy(int[] ratings) {
        
        int n = ratings.length;

        int[] left = new int[n];
        int[] right = new int[n];
        
        left[0] = 1;
        for(int i=1; i<n; i++){
            if(ratings[i] > ratings[i-1]){
                left[i] = left[i-1] + 1;
            }else{
                left[i] = 1;
            }
        }

        right[n-1] = 1;
        for(int i=n-2; i>=0; i--){
            if(ratings[i] > ratings[i+1]){
                right[i] = right[i+1] + 1;
            }else{
                right[i] = 1;
            }
        }

        int count = 0;

        for(int i=0; i<n; i++){
            count += Math.max(left[i], right[i]);
        }

        return count;  
    }
}
```
