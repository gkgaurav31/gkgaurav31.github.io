---
layout: post
title: Pascal's Triangle
date: 2024-03-23 23:12 +0530
author: "Gaurav Kumar"
tags: "java dynamic_programming leetcode"
categories: "dynamic_programming"
---

## PROBLEM DESCRIPTION

Given an integer numRows, return the first numRows of Pascal's triangle.
In Pascal's triangle, each number is the sum of the two numbers directly above it.

[leetcode](https://leetcode.com/problems/pascals-triangle/description/)

## SOLUTION

```java
class Solution {

    public List<List<Integer>> generate(int numRows) {

        // Initialize a list to store the result
        List<List<Integer>> res = new ArrayList<>();

        // Add the first row of Pascal's triangle (just a single 1)
        res.add(new ArrayList<>(Arrays.asList(1)));

        // Iterate through each row
        for (int i = 1; i < numRows; i++) {

            // Initialize a list to store the current row
            List<Integer> current = new ArrayList<>();

            // Add the first element of the current row (always 1)
            current.add(1);

            // Get the previous row from the result list
            List<Integer> prev = res.get(res.size() - 1);

            // Iterate through each element of the previous row (excluding the last one)
            for (int j = 0; j < prev.size() - 1; j++) {

                // Calculate the next element of the current row by summing up adjacent elements of the previous row
                current.add(prev.get(j) + prev.get(j + 1));

            }

            // Add the last element of the current row (always 1)
            current.add(1);

            // Add the current row to the result list
            res.add(current);

        }

        return res;
    }
}
```
