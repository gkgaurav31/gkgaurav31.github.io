---
layout: post
title: Combinations
date: 2023-08-19 20:57 +0530
author: "Gaurav Kumar"
tags: "java leetcode leetcode150 recursion backtracking"
categories: "backtracking"
---

## Problem Description

Given two integers n and k, return all possible combinations of k numbers chosen from the range [1, n].
You may return the answer in any order.

Example:

```text
Input: n = 4, k = 2
Output: [[1,2],[1,3],[1,4],[2,3],[2,4],[3,4]]
Explanation: There are 4 choose 2 = 6 total combinations.
Note that combinations are unordered, i.e., [1,2] and [2,1] are considered to be the same combination.
```

[leetcode](https://leetcode.com/problems/combinations/)

## Solution

```java
class Solution {

    public List<List<Integer>> combine(int n, int k) {
        return combineHelper(n, k, 1, new ArrayList<Integer>(), new ArrayList<List<Integer>>());
    }

    //recursive helper that generates all combinations.
    public List<List<Integer>> combineHelper(int n, int k, int idx, List<Integer> currentList, List<List<Integer>> all){

        // Base case: If the current combination size reaches 'k', add it to the result list 'all'.
        if(currentList.size() == k){
            all.add(new ArrayList<>(currentList)); //make sure to create a new array and all that to the complete list
            return all;
        }

        // Iterate over possible numbers that can be included in the combination.
        for(int i=idx; i<=n; i++){

            // Add the current number to the combination.
            currentList.add(i);

            // Recursively generate combinations by moving to the next index and considering the updated currentList.
            combineHelper(n, k, i+1, currentList, all);

            // Backtrack: Remove the last added number to explore other possibilities.
            currentList.remove(currentList.size()-1);

        }

        return all;
    }
}
```
