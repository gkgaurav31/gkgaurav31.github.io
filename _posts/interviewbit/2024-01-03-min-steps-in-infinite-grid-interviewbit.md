---
layout: post
title: Min Steps in Infinite Grid (Interviewbit)
date: 2024-01-03 22:29 +0530
uthor: "Gaurav Kumar"
tags: "java arrays interviewbit"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

You are in an infinite 2D grid where you can move in any of the 8 directions (we can move diagonally as well)

You are given a sequence of points and the order in which you need to cover the points.Give the minimum number of steps in which you can achieve it. You start from the first point.

[interviewbit](https://www.interviewbit.com/problems/occurence-of-each-number/)

## SOLUTION

Imagine we're moving on a grid where we can go in any direction, including diagonally. When we have two points, what really matters is how far apart they are in terms of their x-coordinates (let's call this 'X') and y-coordinates (let's call this 'Y').

We move diagonally whenever we can, reducing both 'X' and 'Y' by 1 in each step.

When 'X' or 'Y' becomes zero, we start moving either horizontally or vertically, taking one step at a time until we reach the final point. So, the total steps needed to cover the distance between two points equals the longer distance between 'X' and 'Y'.

```java
public class Solution {

    public int coverPoints(int[] A, int[] B) {

        int n = A.length;

        int totalSteps = 0;

        for(int i=1; i<n; i++){

            int x1 = A[i-1];
            int y1 = B[i-1];

            int x2 = A[i];
            int y2 = B[i];

            totalSteps += stepsNeeded(x1, y1, x2, y2);

        }

        return totalSteps;

    }

    public int stepsNeeded(int x1, int y1, int x2, int y2){
        return Math.max(Math.abs(x2-x1), Math.abs(y2-y1));
    }

}
```
