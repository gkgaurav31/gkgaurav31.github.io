---
layout: post
title: Noble Integer
date: 2022-10-23 10:49 +0530
author: "Gaurav Kumar"
tags: "java arrays"
categories: "arrays"
---

## Problem Description

Given an integer array A, find if an integer p exists in the array such that the number of integers greater than p in the array equals to p.

### Solution

```java
public class Solution {
    
    public int solve(ArrayList<Integer> A) {
        
        Collections.sort(A);
        
        int n = A.size();
        
        if(A.get(n-1) == 0) return 1; //Edge case, if the last element is 0
        
        for(int i=0; i<n-1; i++){
            
            int p = A.get(i);
            int right = n-i-1;
            
            //current element == number of elements on its right
            //The list can also have duplicates. If the number of its right is a duplicate, we cannot include that since we are looking for stricly greater
            if(p == right && p != A.get(i+1)) return 1; 
        } 
        
        return -1;
        
    }
}
```
