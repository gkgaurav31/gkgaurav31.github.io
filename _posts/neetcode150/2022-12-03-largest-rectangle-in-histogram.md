---
layout: post
title: Largest Rectangle in Histogram
date: 2022-12-03 15:45 +0530
author: "Gaurav Kumar"
tags: "java stack important neetcode150"
categories: "neetcode150"
---

## Problem Description

Given an array of integers heights representing the histogram's bar height where the width of each bar is 1, return the area of the largest rectangle in the histogram.

[leetcode](https://leetcode.com/problems/largest-rectangle-in-histogram/description/)

### Solution

![snapshot]({{ site.baseurl }}/assets/img/largest_rectangle_in_histogram.png)

```java
class Solution {
    
    public int largestRectangleArea(int[] heights) {

        int n = heights.length;

        //store the index of the graph which is nearest smallest than the current bar on the left side
        int[] nearestOnLeft = getNearestSmallerOnLeftIndex(heights);

        //store the index of the graph which is nearest smallest than the current bar on the right side
        int[] nearestOnRight = getNearestSmallerOnRightIndex(heights);

        //init
        int maxArea = 0;

        //loop through all the bars
        for(int i=0; i<n; i++){
            
            //current height of the bar
            int h = heights[i];

            //init
            int leftArea = 0;
            int rightArea = 0;

            //if there is no bar on left which is smaller than the current one, we can consider everything towards left
            if(nearestOnLeft[i] == -1){
                leftArea = h * (i+1);
            //else, multiply current height by distance from the nearest bar which has smaller size on left 
            }else{
                leftArea = h * (i-nearestOnLeft[i]);
            }

            //if there is no bar on right which is smaller than the current one, we can consider everything towards right
            if(nearestOnRight[i] == -1){
                rightArea = h * (n-i);
            //else, multiply current height by distance from the nearest bar which has smaller size on right 
            }else{
                rightArea = h * (nearestOnRight[i]-i);
            }

            //the current bar will be included in both leftArea and rightArea, so remove that overlap
            int currentTotal = leftArea + rightArea - h;

            //update maxArea possible till now
            maxArea = Math.max(maxArea, currentTotal);

        }
        
        return maxArea;
        
    }

    private static int[] getNearestSmallerOnRightIndex(int[] a) {

        int n = a.length;
        int[] nearestSmaller = new int[n];

        Arrays.fill(nearestSmaller, -1);

        Stack<Integer> stack = new Stack<>();
        stack.push(n-1);

        for(int i=n-2; i>=0; i--){
            if(a[stack.peek()] < a[i]){
                nearestSmaller[i] = stack.peek();
                stack.push(i);
            }else{
                while(!stack.isEmpty() && a[stack.peek()] >= a[i]){
                    stack.pop();
                }

                if(!stack.isEmpty()){
                    nearestSmaller[i] = stack.peek();
                }//else it will be -1

                stack.push(i);

            }
        }

        return nearestSmaller;

    }

    private static int[] getNearestSmallerOnLeftIndex(int[] a) {

        int n = a.length;
        int[] nearestSmaller = new int[n];

        Arrays.fill(nearestSmaller, -1);

        Stack<Integer> stack = new Stack<>();
        stack.push(0);

        for(int i=1; i<n; i++){
            if(a[stack.peek()] < a[i]){
                nearestSmaller[i] = stack.peek();
                stack.push(i);
            }else{
                while(!stack.isEmpty() && a[stack.peek()] >= a[i]){
                    stack.pop();
                }

                if(!stack.isEmpty()){
                    nearestSmaller[i] = stack.peek();
                }//else it will be -1

                stack.push(i);

            }
        }

        return nearestSmaller;

    }


}
```
