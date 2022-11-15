---
layout: post
title: Min Triangle Sum
date: 2022-11-16 00:05 +0530
tags: "java leetcode dynamic_programming"
categories: "dynamic_programming"
---

## Problem Description

Given a triangle array, return the minimum path sum from top to bottom.  

For each step, you may move to an adjacent number of the row below. More formally, if you are on index i on the current row, you may move to either index i or index i + 1 on the next row.  

[leetcode](https://leetcode.com/problems/triangle/description/)  

### Solution

We can solve this problem in a similar way as we solved [Paint House]({% post_url 2022-11-07-paint-house %}).

The first row has only one element, and that's the minimum sum possible till row 0.
For row 1, if we pick the first element, we can choose between the element right above it or the one before it. We can consider them one by one and take the minimum. So now we have the minimum sum possible till row 1. In the same way, we calculate the min sum for row 2 onwards. The final answer is the minimum number in the last row of the dp.

```java
class Solution {

    public int minimumTotal(List<List<Integer>> a) {
   
        //numbers of rows
        int n = a.size(); 

        //size of last row
        int m = a.get(a.size()-1).size(); 

        int[][] dp = new int[n][m];

        //initialize
        dp[0][0] = a.get(0).get(0);

        for(int i=1; i<a.size(); i++){

            for(int j=0; j<a.get(i).size(); j++){

                int x = Integer.MAX_VALUE;
                int y = Integer.MAX_VALUE;
                
                int current = a.get(i).get(j);

                //previous column (in the previous row)
                if(j-1 >= 0){
                    x = dp[i-1][j-1] + current;
                }

                //same column (in the previous row)
                if( j <= a.get(i-1).size()-1){
                    y = dp[i-1][j] + current;
                }

                dp[i][j] = Math.min(x,y);

            }
        }

        int ans = Integer.MAX_VALUE;
        for(int i=0; i<m; i++){
            ans = Math.min(dp[n-1][i], ans);
        }

        return ans;

    }
}
```
