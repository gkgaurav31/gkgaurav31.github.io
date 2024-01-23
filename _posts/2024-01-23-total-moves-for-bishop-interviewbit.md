---
layout: post
title: Total Moves For Bishop! (InterviewBit)
date: 2024-01-23 21:59 +0530
author: "Gaurav Kumar"
tags: "java interviewbit mathematics"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given the position of a Bishop (A, B) on an `8 x 8` chessboard.

Your task is to count the total number of squares that can be visited by the Bishop in one move.

The position of the Bishop is denoted using row and column number of the chessboard.

[InterviewBit](https://www.interviewbit.com/problems/total-moves-for-bishop/)

## SOLUTION

Bishop can only move diagonally.

```java
public class Solution {

    public int solve(int A, int B) {

        // change to 0 index
        A = A-1;
        B = B-1;

        // top left
        // limited by top rows and left columns
        int tl = Math.min(A, B);

        // top right
        // limited by top rows and right columns
        int tr = Math.min(A, 8-B-1);

        // bottom left
        // limited by bottom rows and left columns
        int bl = Math.min(B, 8-A-1);

        // bottom right
        // limited by bottom rows and right columns
        int br = Math.min(8-B-1, 8-A-1);

        // total squares possible
        return tl + tr + bl + br;

    }

}
```
