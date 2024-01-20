---
layout: post
title: Pascal Triangle (InterviewBit)
date: 2024-01-20 14:41 +0530
author: "Gaurav Kumar"
tags: "java interviewbit arrays"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given an integer `A`, equal to numRows, generate the first numRows of Pascal's triangle.

Pascal's triangle: To generate `A[C]` in row `R`, sum up `A'[C]` and `A'[C-1]` from the previous row `R - 1`.

[InterviewBit](https://www.interviewbit.com/problems/pascal-triangle/)

## SOLUTION

`A[row][col] = A[row-1][col-1] + A[row-1][col]`

This is pretty straight forward, the only thing to take care of is that the column can be out of bounds when looking into the previous row.

```java
public class Solution {

    public ArrayList<ArrayList<Integer>> solve(int A) {

        int n = A;

        ArrayList<ArrayList<Integer>> triangle = new ArrayList<>();

        if(n == 0) return triangle;

        ArrayList<Integer> firstRow = new ArrayList<>();
        firstRow.add(1);

        triangle.add(firstRow);

        for(int r=1; r<n; r++){

            ArrayList<Integer> row = new ArrayList<>();

            for(int c=0; c<=r; c++){

                int nextVal = 0;
                int previousRow = r-1;

                // 1
                // x y
                // a b c
                //the left column of x will be out of bounds, so handle that
                if(c-1 >= 0)
                    nextVal += triangle.get(previousRow).get(c-1);

                // 1
                // x y
                // a b c
                //the column above the last element in each row will be out of bounds for the previous row, so handle that
                if(c < triangle.get(previousRow).size())
                    nextVal += triangle.get(previousRow).get(c);

                row.add(nextVal);

            }

            triangle.add(row);

        }

        return triangle;

    }
}
```
