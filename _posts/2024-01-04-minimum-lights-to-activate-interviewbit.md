---
layout: post
title: Minimum Lights to Activate (InterviewBit)
date: 2024-01-04 22:38 +0530
Author: "Gaurav Kumar"
tags: "java arrays greedy interviewbit"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

There is a corridor in a Jail which is N units long. Given an array A of size N. The ith index of this array is 0 if the light at ith position is faulty otherwise it is 1.

All the lights are of specific power B which if is placed at position X, it can light the corridor from [ X-B+1, X+B-1].

Initially all lights are off.

Return the minimum number of lights to be turned ON to light the whole corridor or -1 if the whole corridor cannot be lighted.

[interviewbit](https://www.interviewbit.com/problems/minimum-lights-to-activate/)

## SOLUTION

[Good Explanation](https://www.youtube.com/watch?v=JG77IVjK8D8)

We start from position 0. Let us say that the power of all bulbs in `B`. So the position 0 bulb covers the range `[0-B+1, 0+B-1]`. The most important thing here is to understand why we would choose the right most _working_ bulb in this range `[0-B+1, 0+B-1]`. If we choose a bulb which is before that, there can be overlap. Also, the right bulb will help light up the bulbs further away from it towards its right. So, we choose the right most _working_ bulb in this range to light up. Increase the count after this is chosen. Let us say, that the bulb at positon `X` was chosen to light up in this range. So, that bulb would have lighted up `[X-B+1, X+B-1]` range. Therefore, we can start checking for next bulb to be lighted up from position `X+B`.

```java
public class Solution {

    public int solve(int[] A, int B) {

        int n = A.length;

        int count = 0; // count of bulbs that will be turned on

        int idx = 0; // Pointer to iterate through the corridor

        // Iterate through the corridor until all parts are covered
        while (idx < n) {

            // Calculate the leftmost and rightmost reachable positions by the current light
            int left = Math.max(0, idx - B + 1);
            int right = Math.min(n - 1, idx + B - 1);

            // Find the rightmost good bulb index in the current range (good bulb will value A[i] == 1)
            int rightMostGoodBulbIndex = rightMostBulb(A, left, right);

            // If no good bulb is found in the given range, return -1 since it will not be possible to light the current position
            if (rightMostGoodBulbIndex == -1)
                return -1;

            count++; // Increment the count for the turned ON light

            // Move the pointer to the position beyond the covered range
            // If we turn on bulb at position X, it will cover [X-B+1 to X+B-1]. So the next position we need to look is after this range which is X+B
            idx = rightMostGoodBulbIndex + B;
        }

        return count;
    }

    // Helper function to find the rightmost good bulb index in the given range
    public int rightMostBulb(int[] A, int left, int right) {

        // Iterate from the right end of the range towards the left
        while (right >= left) {

            // If a working bulb is found, return its index
            if (A[right] == 1) {
                return right;
            }

            right--; // Move leftward within the range to look for possible next working bulb
        }
        return -1; // If no working bulb is found in the range, return -1
    }
}
```
