---
layout: post
title: Find Square Root (Binary Search Approach)
date: 2022-08-06 02:27 +0530
author: "Gaurav Kumar"
tags: "java arrays search important"
categories: "search"

---

## Problem Description

Given a non-negative integer x, compute and return the square root of x.
[Leetcode: Sqrt(x)](https://leetcode.com/problems/sqrtx/)

Constraints:
> 0 <= x <= 2^31 - 1


## Solution

The important part in this code is to realize that m*m can lead to overflow. To handle this, we can do:
> m == x/m

```java
class Solution {
    
    public int mySqrt(int x) {
        
        int l=1, h=x, root=0;
        
        while(l<=h){
            
            int m = (l+h)/2;
            
            if(m == x/m) return m; //m*m can lead to overflow. 
            
            if(m>x/m){
                h = m-1;
            }else{
                root = m;
                l = m+1;
            }
                
        }
        
        return root;
        
    }
    
}
```
