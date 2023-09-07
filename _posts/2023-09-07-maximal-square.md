---
layout: post
title: Maximal Square
date: 2023-09-07 22:31 +0530
author: "Gaurav Kumar"
tags: "java dynamic_programming leetcode leetcode150 important"
categories: "dynamic_programming"
---

## PROBLEM DESCRIPTION

Given an m x n binary matrix filled with 0's and 1's, find the largest square containing only 1's and return its area.
[leetcode](https://leetcode.com/problems/maximal-square)

## SOLUTION

[Neetcode YouTube](https://www.youtube.com/watch?v=6X7Ha2PrDmM)

```java
class Solution {

    public int maximalSquare(char[][] matrix) {
        // Create a map to store the results of subproblems
        Map<String, Integer> map = new HashMap<>();

        // Iterate through each cell in the matrix
        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < matrix[0].length; j++) {
                // Call the helper function to calculate the maximum square
                helper(matrix, i, j, map);
            }
        }

        // Find the maximum side length of the square from the map
        int maxLength = Collections.max(map.values());

        // Return the area of the largest square
        return maxLength * maxLength;
    }

    public int helper(char[][] matrix, int r, int c, Map<String, Integer> map) {
        // If the current cell is out of bounds, return 0
        if (r >= matrix.length || c >= matrix[0].length) {
            return 0;
        }

        // Create a unique key for the current cell
        String key = r + ":" + c;

        // If the result for this cell is not already calculated, calculate it
        if (!map.containsKey(key)) {
            // If the current cell contains '1', calculate the maximum square
            if (matrix[r][c] == '1') {
                // Calculate the values of adjacent cells and the diagonal cell
                int down = helper(matrix, r + 1, c, map);
                int right = helper(matrix, r, c + 1, map);
                int diagonal = helper(matrix, r + 1, c + 1, map);

                // Find the minimum of these values
                int min = Math.min(down, Math.min(right, diagonal));

                // Store the result in the map, adding 1 to it
                map.put(key, 1 + min);
            } else {
                // If the current cell contains '0', set the result to 0
                map.put(key, 0);
            }
        }

        // Return the result for the current cell
        return map.get(key);
    }
}
```
