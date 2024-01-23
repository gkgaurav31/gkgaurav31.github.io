---
layout: post
title: Distribute in Circle! (InterviewBit)
date: 2024-01-23 22:20 +0530
author: "Gaurav Kumar"
tags: "java interviewbit mathematics"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

`A` items are to be delivered in a circle of size `B`.

Find the position where the `Ath` item will be delivered if we start from a given position `C`.

NOTE: Items are distributed at adjacent positions starting from `C`.

[InterviewBit](https://www.interviewbit.com/problems/total-moves-for-bishop/)

## SOLUTION

```java
public class Solution {
    public int solve(int A, int B, int C) {

        C = (C-1)%B; // conver to 0 index
        A = A%B; // after every B items, it will reach same position, so A%B will give extra items left

        return (C + A)%B; // from C, deliver more A items to reach the needed position (if it goes beyond B, it will cycle again)

    }
}
```

> Alternative: return (A+C-1)%B;
