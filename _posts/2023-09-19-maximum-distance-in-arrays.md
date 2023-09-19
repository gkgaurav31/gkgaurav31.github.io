---
layout: post
title: Maximum Distance in Arrays
date: 2023-09-19 21:45 +0530
author: "Gaurav Kumar"
tags: "java arrays leetcode leetcodealgo100"
categories: "arrays"
---

## PROBLEM DESCRIPTION

You are given m arrays, where each array is sorted in ascending order.
You can pick up two integers from two different arrays (each array picks one) and calculate the distance. We define the distance between two integers a and b to be their absolute difference |a - b|.

Return the maximum distance.

[leetcode](https://leetcode.com/problems/maximum-distance-in-arrays/)

## SOLUTION

### BRUTE-FORCE (TLE)

```java
class Solution {

    public int maxDistance(List<List<Integer>> arrays) {

        int n = arrays.size();

        // Create arrays to store the minimum and maximum values from each input array
        int[] minArr = new int[n];
        int[] maxArr = new int[n];

        // Populate minArr and maxArr with the minimum and maximum values from each input array
        for(int i=0; i<n; i++){
            minArr[i] = arrays.get(i).get(0); // Store the first element as the minimum
            maxArr[i] = arrays.get(i).get(arrays.get(i).size()-1); // Store the last element as the maximum
        }

        // Initialize a variable to keep track of the maximum distance
        int maxDistance = 0;

        // Nested loops to compare pairs of arrays and calculate the maximum distance
        for(int i=0; i<n; i++){

            for(int j=i+1; j<n; j++){

                // Calculate the distance between the minimum of one array and the maximum of another
                maxDistance = Math.max(maxDistance, Math.abs(minArr[i]-maxArr[j]));

                // Calculate the distance between the maximum of one array and the minimum of another
                maxDistance = Math.max(maxDistance, Math.abs(maxArr[i]-minArr[j]));

            }
        }

        return maxDistance;
    }
}
```

### OPTIMIZED USING PREFIX/SUFFIX ARRAY

At any index, the first element will be the smallest. We want to maximize the distance by picking the maximum number which can be either on its left or right. We can use prefix and suffix array for this purpose, which will be created based on the maximum-array which contains the max value for that position. So, to find the distance at position x, we know the min at x and we need to look for max on left lMax[x-1] or right rMax[x+1] and choose whichever is the larger one.

```java
class Solution {

    public int maxDistance(List<List<Integer>> arrays) {

        int n = arrays.size();

        // Create arrays to store the minimum and maximum values from each input array
        int[] minArr = new int[n];
        int[] maxArr = new int[n];

        // Populate minArr and maxArr with the minimum and maximum values from each input array
        for(int i=0; i<n; i++){
            minArr[i] = arrays.get(i).get(0); // Store the first element as the minimum
            maxArr[i] = arrays.get(i).get(arrays.get(i).size()-1); // Store the last element as the maximum
        }

        // Create prefix and suffix arrays to store the maximum values to the left and right of each array
        // We can also choose to do this for minArray instead of maxArray. The idea will be the same.
        int[] lMax = new int[n]; // lMax[i] stores the maximum value to the left of array i, including the current element
        int[] rMax = new int[n]; // rMax[i] stores the maximum value to the right of array i, including the current element

        // Initialize the first elements of lMax and the last elements of rMax
        lMax[0] = maxArr[0];
        rMax[n-1] = maxArr[n-1];

        // Calculate the maximum values to the left of each array
        for(int i=1; i<n; i++){
            lMax[i] = Math.max(lMax[i-1], maxArr[i]);
        }

        // Calculate the maximum values to the right of each array
        for(int i=n-2; i>=0; i--){
            rMax[i] = Math.max(rMax[i+1], maxArr[i]);
        }

        // Initialize a variable to keep track of the maximum distance
        int maxDistance = 0;

        // Iterate through each array and calculate the maximum distance for each
        for(int i=0; i<n; i++){

            int min = minArr[i]; // Get the minimum value from the current array

            int max = Integer.MIN_VALUE;

            // Determine the maximum value from either the left or right neighbor arrays,
            // or if the current array is at the boundaries, only consider one neighbor.
            if(i == 0){
                max = rMax[1];
            }else if(i == n-1){
                max = lMax[n-2];
            }else{
                max = Math.max(lMax[i-1], rMax[i+1]);
            }

            // Calculate and update the maximum distance
            maxDistance = Math.max(maxDistance, Math.abs(max-min));

        }

        return maxDistance;

    }

}
```
