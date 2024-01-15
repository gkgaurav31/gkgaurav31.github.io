---
layout: post
title: Max Distance (InterviewBit)
date: 2024-01-15 21:27 +0530
author: "Gaurav Kumar"
tags: "java interviewbit arrays"
categories: "interviewbit"
---

## Problem Description

Given an array A of integers, find the maximum of `j - i` subjected to the constraint of `A[i] <= A[j]`.

[InterviewBit](https://www.interviewbit.com/problems/max-distance/)

### Solution

**Intuition:**

For any element, if we know the largest index possible on its right side, we have a candidate for the maximum value.

For Example: [3 5 4 2]  
For 3, the numbers which are larger than 3 are 5 and 4. Between 5 and 4, the index which will be larger will be further away, in this case index 2.

If we had a sorted array, we know which values are larger than it. But the problem is that, we would lose the indices of those numbers.

So, first of all, we create a new array which has the number along with its index. Then, we sort the array using the number (in ascending order).

So now, we can start from left to right. At every position, we have the current index value and all the numbers towards its right are greater than it.

This is still not optimal because we would need to check all numbers greater than the current element one by one.

So, we need a way to _find the maximum index possible on the right side of current element_ (Think suffix array!).

By creating the suffix array to store the largest indices from right to left, we have all three things that we require:

- Sorted numbers (so that we can start from smallest to largest number)
- Index of each number
- Largest index among the numbers which are larger than the current element (most important piece)

All we need to do is update the maximum difference `j-i` using:  
`maxDiff = Math.max(maxDiff, maxIndexOnRight - actualIndex)`

```java
public class Solution {

    public int maximumGap(final int[] A) {

        int n = A.length;

        // Base cases: If the array is empty or has only one element, return 0
        if(n == 0 || n == 1) return 0;

        // Create an array of Pair objects to store each element along with its index
        Pair[] pairs = new Pair[n]; //Pair -> [number, index]

        // Populate the pairs array with elements and their corresponding indices
        for(int i=0; i<n; i++){
            pairs[i] = new Pair(A[i], i);
        }

        // Sort the pairs based on the values of the elements
        Arrays.sort(pairs, (a,b) -> a.num - b.num);

        // Create an array to store the maximum index on the right side for each element
        int[] sf = new int[n];

        sf[n-1] = Integer.MIN_VALUE; //initialize with min value, since we will be using max to find the maximum index value from right to left

        // Populate the sf array with the maximum index on the right for each element
        for(int i=n-2; i>=0; i--){
            sf[i] = Math.max(sf[i+1], pairs[i+1].index);
        }

        // Initialize the maximum difference variable to track the result
        int maxDiff = Integer.MIN_VALUE;

        // Iterate through the pairs to find the maximum difference
        for(int i=0; i<n-1; i++){

           // Get the actual index of the current element
           int actualIndex = pairs[i].index;

           // Get the maximum index on the right side for the current element
           int maxIndexOnRight = sf[i];

           // Update the maximum difference if a larger difference is found
           maxDiff = Math.max(maxDiff, maxIndexOnRight - actualIndex);

        }

        // Return the maximum difference, ensuring it is non-negative
        // Example [3 2 1] -> There is no number which has a higher value on its right
        return Math.max(0, maxDiff);

    }
}

// Custom class to represent a pair of (element, index)
class Pair{

    int num;    // Element value
    int index;  // Element index

    Pair(int num, int index){
        this.num = num;
        this.index = index;
    }
}
```
