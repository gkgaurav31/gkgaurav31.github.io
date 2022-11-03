---
layout: post
title: Minimum Number of Squares
date: 2022-11-04 00:19 +0530
author: "Gaurav Kumar"
tags: "java dynamic_programming"
categories: "dynamic_programming"
---

## Problem Description

Given an integer A. Return minimum count of numbers, sum of whose squares is equal to A.

### Solution

```java
public class Solution {
    public int countMinSquares(int A) {

        int[] dp = new int[A+1];

        dp[1] = 1;

        //Loop through each number and save its answer in dp array
        for(int i=2; i<= A; i++){
            
            //Initialize answer for i as i which is its max possible answer with all 1s.
            int ans = i;

            //What we are essentially doing here is, subtracting 1^2, 2^2, 3^2, 4^2 and so on, from the current number i
            //So, the answer for i is: 1+dp[number minus square of j]
            //Adding one because we have subtracted one number which would be part of the answer
            for(int j=1; j*j<=i; j++){
                ans = Math.min(ans, dp[i-(j*j)]);
            }

            dp[i] = ans+1; 

        }

        return dp[A];

    }
}
```
