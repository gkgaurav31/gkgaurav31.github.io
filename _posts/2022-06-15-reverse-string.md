---
layout: post
title: Reverse String
date: 2022-06-15 02:04 +0530
author: "Gaurav Kumar"
tags: "java strings"
categories: "leetcode"

---

## Problem Description

Given a character array, reverse it.  
[Reverse String](https://leetcode.com/problems/reverse-string/solution/)

## Solution

We can use recursion for this, although it will __not__ be O(1) space complexity. The other approach is to use two pointers - front and back. Each time swap the two elements and keep moving to the inner elements.

### Recursive Solution  

```java
class Solution {
    public void reverseString(char[] s) {        
        if(s.length <= 1) return;
        reverseStringX(s, 0, s.length-1);
    }
    
    public void reverseStringX(char[] s, int start, int end) {
        
        if(start>=end) return;
        
        char temp = s[start];
        s[start] = s[end];
        s[end] = temp;
        
        reverseStringX(s, start+1, end-1);
        
    }
}
```
