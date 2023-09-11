---
layout: post
title: Max Points on a Line
date: 2023-09-11 20:27 +0530
author: "Gaurav Kumar"
tags: "java mathematics leetcode leetcode150"
categories: "mathematics"
---

## PROBLEM DESCRIPTION

Given an array of points where points[i] = [xi, yi] represents a point on the X-Y plane, return the maximum number of points that lie on the same straight line.  
[leetcode](https://leetcode.com/problems/max-points-on-a-line/)

## SOLUTION

[Neetcode YouTube](https://www.youtube.com/watch?v=Bb9lOXUOnFw)

```java
import java.util.HashMap;
import java.util.Map;

class Solution {

    public int maxPoints(int[][] points) {

        int maxLength = 1; // Initialize the maximum length of points on the same line to 1 (minimum possible)

        for (int i = 0; i < points.length; i++) {

            //fix the first point
            int[] p1 = points[i];
            int x1 = p1[0];
            int y1 = p1[1];

            // Create a map to store slopes and their counts (with other points)
            Map<Double, Integer> map = new HashMap<>();

            for (int j = i + 1; j < points.length; j++) {

                int[] p2 = points[j];

                int x2 = p2[0];
                int y2 = p2[1];

                double m = 0; // Initialize the slope to 0

                if (x1 == x2) {
                    // If the x-coordinates are the same, the line is vertical (infinite slope)
                    m = Double.POSITIVE_INFINITY;
                } else if (y1 == y2) {
                    // If the y-coordinates are the same, the line is horizontal (slope is 0)
                    m = 0;
                } else {
                    // Calculate the slope for non-vertical and non-horizontal lines
                    m = (double) (y2 - y1) / (x2 - x1);
                }

                // Store the slope in the map along with its count (how many points share the same slope)
                map.put(m, map.getOrDefault(m, 0) + 1);

                // Update the maximum length of points on the same line
                // We are adding one because the number points will be one more. The HashMap contains frequency for slope x.
                maxLength = Math.max(maxLength, map.get(m) + 1);
            }
        }

        return maxLength;
    }
}
```
