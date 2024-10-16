---
layout: post
title: Rotate matrix elements clockwise
date: 2024-10-16 17:02 +0530
author: "Gaurav Kumar"
tags: "java arrays geeksforgeeks sde-sheet important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given two integers M, N, and a 2D matrix Mat of dimensions MxN, clockwise rotate the elements in it.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/rotate-matrix-elements-clockwise2336/1?page=4)

## SOLUTION

- We handle the matrix layer by layer, starting by defining the top, bottom, left, and right boundaries, which will keep updating as we move inward to the inner layers.
- The main idea is to store the element just below the top-left corner for rotation and use that while swapping with other elements.
  - First, store `arr[topRow+1][leftColumn]` in a `prev` variable.
  - Next, iterate from `leftColumn` to `rightColumn`.  
    For each position:
    - save the current value in a `temp` variable
    - place `prev` in the current position
    - update `prev` to the current element to be used for the next position.
  - Do the same for other side as well, right, bottom and left.
  - Keep repeating for remaining layers.

```java
class Solution {

    int[][] rotateMatrix(int n, int m, int arr[][]) {

        int top = 0;
        int bottom = n - 1;
        int left = 0;
        int right = m - 1;

        // Loop until we finish rotating all layers
        while (top < bottom && left < right) {

            // Store the element just below the top-left corner for rotation
            int prev = arr[top + 1][left];

            // Rotate the top row from left to right
            for (int c = left; c <= right; c++) {
                // Temporarily store the current element
                int temp = arr[top][c];

                // Replace current element with the previous element
                arr[top][c] = prev;

                // Update prev to hold the current element for the next iteration
                prev = temp;
            }

            // Rotate the right column from top to bottom
            for (int r = top + 1; r <= bottom; r++) {
                // Temporarily store the current element
                int temp = arr[r][right];

                // Replace current element with the previous element
                arr[r][right] = prev;

                // Update prev to hold the current element for the next iteration
                prev = temp;
            }

            // Rotate the bottom row from right to left
            for (int c = right - 1; c >= left; c--) {
                // Temporarily store the current element
                int temp = arr[bottom][c];

                // Replace current element with the previous element
                arr[bottom][c] = prev;

                // Update prev to hold the current element for the next iteration
                prev = temp;
            }

            // Rotate the left column from bottom to top
            for (int r = bottom - 1; r >= top + 1; r--) {
                // Temporarily store the current element
                int temp = arr[r][left];

                // Replace current element with the previous element
                arr[r][left] = prev;

                // Update prev to hold the current element for the next iteration
                prev = temp;
            }

            // Move the top boundary down to the next layer
            top++;

            // Move the bottom boundary up to the next layer
            bottom--;

            // Move the left boundary right to the next layer
            left++;

            // Move the right boundary left to the next layer
            right--;
        }

        // Return the rotated matrix
        return arr;
    }

}
```
