---
layout: post
title: Matrix Median (InterviewBit)
date: 2024-02-11 13:16 +0530
tags: "java binary_search"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given a matrix of integers `A` of size `N x M` in which each row is sorted.

Find and return the overall median of matrix `A`.

NOTE: No extra memory is allowed.

NOTE: Rows are numbered from top to bottom and columns are numbered from left to right.

## SOLUTION

[Good Explanation - take U forward on YouTube](https://www.youtube.com/watch?v=Q9wXgdxJq48)

This can be solved using binary search. The search range will be between the minimum number of the matrix to largest number in the matrix.

Thought process:

If we pick a middle number `mid`, how can we determine if it is the median? What is we know how many numbers are present in the matrix which are <= `mid`? If we think about this, it should be >= position of the median. The tricky part is to understand that this will be satified by all numbers from median to any number after that. So, using binary search, we should find the first such a number which satifies this condition - which means, the number of elements <= `mid` must be >= the position of the median. Key thing is to find the first occurance of such a number.

```java
public class Solution {

    public int findMedian(int[][] A) {

        int n = A.length; // Number of rows
        int m = A[0].length; // Number of columns

        // Search range will be between least and maximum number in the matrix
        // Get the minimum value from the first column
        int l = getLeft(A);

        // Get the maximum value from the last column
        int r = getRight(A);

        // Calculate the position of the median
        int medianPosition = ((n * m) + 1) / 2;

        int ans = -1; // Initialize answer variable

        // Perform binary search to find the median
        while (l <= r) {

            // middle element
            int mid = (l + r) / 2;

            // Count the number of elements less than or equal to mid
            int countLess = countLess(A, mid);

            // If the count of elements less than or equal to mid is greater than or equal to medianPosition
            // If it a possible answer, so save it
            // But we need to look further on the left because there can be other number which also has a countLess >= medianPosition (in which case, that will be the median)
            if (countLess >= medianPosition) {
                r = mid - 1;
                ans = mid;
            } else {
                l = mid + 1;
            }
        }

        return ans;
    }

    // Method to count the number of elements less than or equal to mid in the matrix
    public int countLess(int[][] A, int mid) {

        int n = A.length; // Number of rows
        int m = A[0].length; // Number of columns

        int totalCount = 0; // Initialize total count

        // Iterate through each row and count elements less than or equal to mid
        for (int r = 0; r < n; r++) {
            totalCount += countLessInRow(A, mid, r); // Update totalCount
        }

        return totalCount;
    }

    // Method to count the number of elements less than or equal to val in a given row
    public int countLessInRow(int[][] A, int val, int row) {

        int n = A.length; // Number of rows
        int m = A[0].length; // Number of columns

        int l = 0; // Initialize left pointer
        int r = m - 1; // Initialize right pointer

        int pos = -1;

        // Perform binary search within the row to find the position of val
        while (l <= r) {

            int mid = (l + r) / 2; // Calculate middle element

            // If the element at mid is less than or equal to val
            // Save the current position and look on the right since there can be duplicates
            if (A[row][mid] <= val) {
                pos = mid;
                l = mid + 1;
            // Otherwise look on the left side
            } else {
                r = mid - 1;
            }
        }

        return pos + 1; // Return the position incremented by 1 (since we need count of numbers less than val)
    }

    // Method to get the minimum value from the first column of the matrix
    public int getLeft(int[][] A) {

        int n = A.length; // Number of rows
        int m = A[0].length; // Number of columns

        int min = A[0][0]; // Initialize min value

        // Iterate through each row to find the minimum value
        for (int r = 0; r < n; r++) {
            min = Math.min(min, A[r][0]); // Update min if necessary
        }

        return min;
    }

    // Method to get the maximum value from the last column of the matrix
    public int getRight(int[][] A) {

        int n = A.length; // Number of rows
        int m = A[0].length; // Number of columns

        int max = A[0][m - 1]; // Initialize max value

        // Iterate through each row to find the maximum value
        for (int r = 0; r < n; r++) {
            max = Math.max(max, A[r][m - 1]); // Update max if necessary
        }

        return max;
    }
}
```
