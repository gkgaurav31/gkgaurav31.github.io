---
layout: post
title: Spiral Order Matrix II (InterviewBit)
date: 2024-01-20 15:36 +0530
author: "Gaurav Kumar"
tags: "java interviewbit arrays"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given an integer `A`, generate a square matrix filled with elements from `1` to `A^2` in spiral order and return the generated square matrix.

[InterviewBit](https://www.interviewbit.com/problems/spiral-order-matrix-ii/)

## SOLUTION

Mark the startRow, endRow, startColumn and endColumn. Then, start iterated over the perimeter in the order: top row -> right column -> bottom row -> left column. Once this is covered, update startRow -> startRow++, endRow -> endRow--, startColumn -> startColumn++ and endColumn -> endColumn-- for the next perimeter.

```java
public class Solution {

    public int[][] generateMatrix(int A) {

        int n = A;

        int[][] matrix = new int[n][n];

        int startRow = 0;
        int endRow = n-1;
        int startColumn = 0;
        int endColumn = n-1;

        int nextVal = 1;

        while(startRow <= endRow && startColumn <= endColumn){

            //top
            for(int i=startColumn; i<=endColumn; i++){
                matrix[startRow][i] = nextVal;
                nextVal++;
            }

            //right
            for(int i=startRow+1; i<=endRow; i++){
                matrix[i][endColumn] = nextVal;
                nextVal++;
            }

            //bottom
            for(int i=endColumn-1; i>=startColumn; i--){
                matrix[endRow][i] = nextVal;
                nextVal++;
            }

            //left
            for(int i=endRow-1; i>=startRow+1; i--){
                matrix[i][startColumn] = nextVal;
                nextVal++;
            }

            startRow++;
            startColumn++;
            endRow--;
            endColumn--;

        }

        return matrix;


    }

}
```
