---
layout: post
title: Histogram Max Rectangular Area (geeksforgeeks - SDE Sheet)
date: 2024-09-15 17:10 +0530
author: "Gaurav Kumar"
tags: "java stack geeksforgeeks sde-sheet important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Find the largest rectangular area possible in a given histogram where the largest rectangle can be made of a number of contiguous bars. For simplicity, assume that all bars have the same width and the width is 1 unit, the height of each bar will be given by the array arr.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/maximum-rectangular-area-in-a-histogram-1587115620/1?page=7)

## SOLUTION

We try to find the maximum area possible by leveraging the full length of the current bar. For a given bar at index i, if leftIdx represents the index of the nearest smaller bar on the left, we can use this to determine the maximum area up to that position. Similarly, if rightIdx denotes the index of the nearest smaller bar on the right, we can calculate the area extending to that index. By combining these two areas—one on the left and one on the right—we can compute the total area where the current bar is the smallest. We continuously update the maximum area encountered by considering each bar as the smallest bar in potential rectangles.

```java
class Solution {

    // Function to find the index of the nearest smaller element on the right of each bar
    public static int[] getRightSmaller(long hist[]){

        int n = hist.length;
        int[] rightSmaller = new int[n];
        Arrays.fill(rightSmaller, -1); // Initialize all elements to -1

        Stack<Integer> stack = new Stack<>();
        stack.push(0); // Start with the first element

        for(int i = 1; i < n; i++){

            long current = hist[i];

            // Pop elements from stack while the current element is smaller than the element at the index on top of the stack
            while(!stack.isEmpty() && current < hist[stack.peek()]){
                rightSmaller[stack.pop()] = i; // Update the index of the nearest smaller element on the right
            }

            stack.push(i); // Push the current element index onto the stack

        }

        return rightSmaller;

    }

    // Function to find the index of the nearest smaller element on the left of each bar
    public static int[] getLeftSmaller(long hist[]){

        int n = hist.length;
        int[] leftSmaller = new int[n];
        Arrays.fill(leftSmaller, -1); // Initialize all elements to -1

        Stack<Integer> stack = new Stack<>();
        stack.push(n - 1); // Start with the last element

        for(int i = n - 2; i >= 0; i--){

            long current = hist[i];

            // Pop elements from stack while the current element is smaller than the element at the index on top of the stack
            while(!stack.isEmpty() && current < hist[stack.peek()]){
                leftSmaller[stack.pop()] = i; // Update the index of the nearest smaller element on the left
            }

            stack.push(i); // Push the current element index onto the stack

        }

        return leftSmaller;

    }

    // Function to find the maximum rectangular area in the histogram
    public static long getMaxArea(long hist[]) {

        int n = hist.length;

        // Get the indices of the nearest smaller elements on the right and left
        int[] rightSmaller = getRightSmaller(hist);
        int[] leftSmaller = getLeftSmaller(hist);

        long maxArea = 0;

        for(int i = 0; i < n; i++){

            long currentArea = 0;

            // if i == 0, leftArea is empty
            if(i == 0){

                int rightIdx = rightSmaller[i];

                // If no smaller element to the right, use the width of the entire histogram
                if(rightIdx == -1){
                    currentArea += (hist[i] * n);
                }else{
                    // Otherwise, use the distance to the nearest smaller element
                    currentArea += (hist[i] * rightIdx);
                }

                maxArea = Math.max(maxArea, currentArea);

            // if i == n-1, rightArea is empty
            } else if (i == n - 1){

                int leftIdx = leftSmaller[i];

                // If no smaller element to the left, use the width of the entire histogram
                if(leftIdx == -1){
                    currentArea += (hist[i] * n);
                // Otherwise, use the distance to the nearest smaller element
                } else {
                    currentArea += (hist[i] * (n - leftIdx - 1));
                }

                maxArea = Math.max(maxArea, currentArea);

            // otherwise calculate both left and right area
            // take care of current bar as overlap
            } else {

                int rightIdx = rightSmaller[i];
                int leftIdx = leftSmaller[i];

                // Calculate the area considering the distance to the nearest smaller element on both sides
                if(rightIdx == -1){
                    currentArea += (hist[i] * (n - i));
                } else {
                    currentArea += (hist[i] * (rightIdx - i));
                }

                if(leftIdx == -1){
                    currentArea += (hist[i] * (i + 1));
                } else {
                    currentArea += (hist[i] * (i - leftIdx));
                }

                currentArea -= hist[i]; // Subtract the overlapping area to avoid double counting

                maxArea = Math.max(maxArea, currentArea);

            }
        }

        return maxArea;
    }

}
```
