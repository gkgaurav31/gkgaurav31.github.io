---
layout: post
title: PRETTYPRINT (InterviewBit)
date: 2024-01-30 20:29 +0530
author: "Gaurav Kumar"
tags: "java mathematics"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Print concentric rectangular pattern in a 2d matrix.

Let us show you some examples to clarify what we mean.

Example 1:

Input: A = 4.

Output:

```text
4 4 4 4 4 4 4
4 3 3 3 3 3 4
4 3 2 2 2 3 4
4 3 2 1 2 3 4
4 3 2 2 2 3 4
4 3 3 3 3 3 4
4 4 4 4 4 4 4
```

## SOLUTION

```java
public class Solution {

    public int[][] prettyPrint(int A) {

        int n = 2*A-1;
        int[][] arr = new int[n][n];

        int current = A;

        int topRow = 0;
        int bottomRow = n-1;
        int leftCol = 0;
        int rightCol = n-1;

        while(topRow <= bottomRow && leftCol <= rightCol){

            for(int c=leftCol; c<=rightCol; c++){
                arr[topRow][c] = current;
            }

            for(int r=topRow; r<=bottomRow; r++){
                arr[r][rightCol] = current;
            }

            for(int c=rightCol; c>=leftCol; c--){
                arr[bottomRow][c] = current;
            }

            for(int r=bottomRow; r>=topRow; r--){
                arr[r][leftCol] = current;
            }

            topRow++;
            bottomRow--;
            leftCol++;
            rightCol--;

            current--;

        }

        return arr;

    }

}
```
