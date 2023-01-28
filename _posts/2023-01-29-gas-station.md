---
layout: post
title: Gas Station
date: 2023-01-29 00:22 +0530
author: "Gaurav Kumar"
tags: "java leetcode greedy neetcode150 important"
categories: "neetcode150"
---

## PROBLEM DESCRIPTION

There are n gas stations along a circular route, where the amount of gas at the ith station is gas[i].

You have a car with an unlimited gas tank and it costs cost[i] of gas to travel from the ith station to its next (i + 1)th station. You begin the journey with an empty tank at one of the gas stations.

Given two integer arrays gas and cost, return the starting gas station's index if you can travel around the circuit once in the clockwise direction, otherwise return -1. If there exists a solution, it is guaranteed to be unique

[leetcode](https://leetcode.com/problems/gas-station/description/)

### SOLUTION

```java
class Solution {

    public int canCompleteCircuit(int[] gas, int[] cost) {
        
        int n = gas.length;

        //if total has is not enough, return -1
        if(Arrays.stream(gas).sum() <  Arrays.stream(cost).sum()) return -1;

        //init
        int remaining = 0;
        int ans = 0;

        //check each station
        for(int i=0; i<n; i++){
            
            //calculate remaining till here
            remaining = remaining + gas[i] - cost[i];
            
            //if remaining fuel is -ve, we cannot reach this position
            //IMPORTANT: next, we will start looking for ans+1
            //It's important to realize that if some point the fuel becomes -ve, no position before that can be the answer
            if(remaining < 0){
                remaining = 0;
                ans = i+1;
            }

        }

        return ans;

    }

}
```
