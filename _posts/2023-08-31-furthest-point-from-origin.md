---
layout: post
title: Furthest Point From Origin
date: 2023-08-31 22:14 +0530
author: "Gaurav Kumar"
tags: "java leetcode contest_question"
categories: "contest_question"
---

## Problem Description

You are given a string moves of length n consisting only of characters 'L', 'R', and '\_'. The string represents your movement on a number line starting from the origin 0.

In the ith move, you can choose one of the following directions:

- move to the left if moves[i] = 'L' or moves[i] = '\_'
- move to the right if moves[i] = 'R' or moves[i] = '\_'

Return the distance from the origin of the furthest point you can get to after n moves.

## Solution

The idea is to use the available empty spaces to balance out the excess of either left or right moves, as these empty spaces can be utilized to move in either direction.

```java
class Solution {

    public int furthestDistanceFromOrigin(String moves) {

        // Initialize counters for left ('L') moves, right ('R') moves, and empty spaces ('_').
        int L = 0;  // Count of left moves
        int R = 0;  // Count of right moves
        int space = 0;  // Count of empty spaces

        // Iterate through each character in the input 'moves' string.
        for (int i = 0; i < moves.length(); i++) {

            char c = moves.charAt(i);  // Get the character at index 'i'.

            // Check the character type and update the corresponding counter.
            if (c == 'L') {
                L++;
            } else if (c == 'R') {
                R++;
            } else if (c == '_') {
                space++;
            }
        }

        // Compare the counts of left and right moves to determine the furthest point.
        if (L >= R) {
            // If there are more or equal left moves than right moves, the furthest point
            // can be reached by moving to the left ('L') as much as possible, and utilizing
            // any available empty spaces ('_') to balance out the extra left moves.
            return L + space - R;  // Final position is the sum of left moves and spaces, minus right moves.
        } else {
            // If there are more right moves than left moves, the furthest point
            // can be reached by moving to the right ('R') as much as possible, and utilizing
            // any available empty spaces ('_') to balance out the extra right moves.
            return R + space - L;  // Final position is the sum of right moves and spaces, minus left moves.
        }
    }

}
```
