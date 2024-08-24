---
layout: post
title: Spirally traversing a matrix (geeksforgeeks - SDE Sheet)
date: 2024-08-24 20:27 +0530
author: "Gaurav Kumar"
tags: "java arrays geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

You are given a rectangular matrix, and your task is to return an array while traversing the matrix in spiral form.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/spirally-traversing-a-matrix-1587115621/1?page=1)

## SOLUTION

```java
class Solution {

    // Function to return a list of integers denoting spiral traversal of matrix.
    public ArrayList<Integer> spirallyTraverse(int matrix[][]) {

        int n = matrix.length;
        int m = matrix[0].length;

        int topRow = 0;
        int bottomRow = n-1;

        int leftColumn = 0;
        int rightColumn = m-1;

        ArrayList<Integer> list = new ArrayList<>();

        while(topRow <= bottomRow && leftColumn <= rightColumn){

            // top
            for(int c=leftColumn; c<=rightColumn; c++){
                list.add(matrix[topRow][c]);
            }

            // right
            for(int r=topRow+1; r<=bottomRow; r++){
                list.add(matrix[r][rightColumn]);
            }

            // bottom
            for(int c=rightColumn-1; c>=leftColumn; c--){
                if(topRow == bottomRow) break;
                list.add(matrix[bottomRow][c]);
            }

            // left
            for(int r=bottomRow-1; r>=topRow+1; r--){
                if(leftColumn == rightColumn) break;
                list.add(matrix[r][leftColumn]);
            }

            topRow++;
            bottomRow--;
            leftColumn++;
            rightColumn--;

        }

        return list;

    }

}
```
