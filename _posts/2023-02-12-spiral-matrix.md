---
layout: post
title: Spiral Matrix
date: 2023-02-12 20:25 +0530
author: "Gaurav Kumar"
tags: "java neetcode150 mathematics geometry important"
categories: "neetcode150"
---

This question is part of [NeetCode150 series](https://neetcode.io/practice).  

## Problem Description

Given an m x n matrix, return all elements of the matrix in spiral order.

[leetcode](https://leetcode.com/problems/spiral-matrix/description/)

### Solution

[Spiral Traversal](https://www.algoexpert.io/questions/spiral-traverse)

```java
class Solution {

    public List<Integer> spiralOrder(int[][] matrix) {

        int n = matrix.length;
        int m = matrix[0].length;

        int startRow = 0;
        int endRow = n-1;
        int startColumn = 0;
        int endColumn = m-1;

        List<Integer> list = new ArrayList<>();

        //print the values at the perimeter, then do the same for the inner rectangles
        while(startRow <= endRow && startColumn <= endColumn){

            for(int i=startColumn; i<=endColumn; i++){
                list.add(matrix[startRow][i]);
            }

            for(int i=startRow+1; i<=endRow; i++){
                list.add(matrix[i][endColumn]);
            }

            for(int i=endColumn-1; i>=startColumn; i--){

                //edge case: when there is a single row in the middle
                //this will be traversed in the first for loop, so we can break if that happens
                if(startRow == endRow) break;
                list.add(matrix[endRow][i]);
            }

            for(int i=endRow-1; i>=startRow+1; i--){

                //edge case: when there is a single column in the middle
                //this will be traversed in the second for loop, so we can break if that happens
                if(startColumn == endColumn) break;
                list.add(matrix[i][startColumn]);
            }

            startRow++; startColumn++;
            endRow--; endColumn--;

        }

        return list;

    }

}
```
