---
layout: post
title: Reverse Bits
date: 2022-10-15 12:34 +0530
author: "Gaurav Kumar"
tags: "java neetcode150"
categories: "neetcode150"
---

This question is part of [NeetCode150 series](https://neetcode.io/practice).  

## Problem Description

Reverse bits of a given 32 bits unsigned integer.  
[leetcode](https://leetcode.com/problems/reverse-bits/)

### Solution

```java
public class Solution {
    
    public int reverseBits(int n) {
    
        int ans = 0;
        
        for(int i=0; i<=31; i++){
            if(isSet(n, i)){
                ans = ans + (1<<(31-i));
            }   
        }
        
        return ans;
        
    }
    
    public boolean isSet(int n, int pos){
        n = n>>pos;
        return ((n&1)==1);
    }
}
```

### ANOTHER WAY TO CODE

```java
public class Solution {

    // you need treat n as an unsigned value
    public int reverseBits(int n) {
        int ans = 0;

        for (int i = 0; i < 32; i++) {
            ans <<= 1;
            ans |= (n & 1);
            n >>= 1;
        }
        return ans;
    }
}

```
