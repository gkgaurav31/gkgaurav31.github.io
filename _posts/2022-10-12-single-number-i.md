---
layout: post
title: Single Number I
date: 2022-10-12 22:40 +0530
author: "Gaurav Kumar"
tags: "java bit_manipulation"
categories: "bit_manipulation"
---

## Problem Description

Given an array of integers A, every element appears twice except for one. Find that integer that occurs once.

### Solution

```java
public int singleNumber(final List<Integer> A) {
    int ans = 0;
    for(int i=0; i<A.size(); i++) ans^= A.get(i);
    return ans;
}
```
