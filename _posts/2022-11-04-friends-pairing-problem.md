---
layout: post
title: Friends Pairing Problem
date: 2022-11-04 01:00 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks dynamic_programming"
categories: "dynamic_programming"
---

## Problem Description

Given N friends, each one can remain single or can be paired up with some other friend. Each friend can be paired only once. Find out the total number of ways in which friends can remain single or can be paired up.  
Note: Since answer can be very large, return your answer mod 10^9+7.  

[geeksforgeeks](https://www.geeksforgeeks.org/friends-pairing-problem/)  

### Solution

![snapshot]({{ site.baseurl }}/assets/img/party_pair.png)

```java
class Solution
{
    
    long M = 1000000007;
    
    public long countFriendsPairings(int n) 
    { 
        long[] dp = new long[n+1];
        dp[0] = 1;
        dp[1] = 1;
        
        for(int i=2; i<=n; i++){
            dp[i] = (dp[i-1]%M + (i-1)*dp[i-2]%M)%M;
        }        
        
        return dp[n];
        
    }
    
}    
```
