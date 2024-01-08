---
layout: post
title: Flip (InterviewBit)
date: 2024-01-08 23:05 +0530
Author: "Gaurav Kumar"
tags: "java arrays interviewbit"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

You are given a binary string A(i.e. with characters 0 and 1) consisting of characters A1, A2, ..., AN. In a single operation, you can choose two indices L and R such that 1 ≤ L ≤ R ≤ N and flip the characters AL, AL+1, ..., AR. By flipping, we mean change character 0 to 1 and vice-versa.

Your aim is to perform ATMOST one operation such that in final string number of 1s is maximised.

If you don't want to perform the operation, return an empty array. Else, return an array consisting of two elements denoting L and R. If there are multiple solutions, return the lexicographically smallest pair of L and R.

NOTE: Pair (a, b) is lexicographically smaller than pair (c, d) if a < c or, if a == c and b < d.

[interviewbit](https://www.interviewbit.com/problems/flip/)

## SOLUTION

> [Good Explanation](https://www.youtube.com/watch?v=Etkf8sRQdbU)

The idea is to convert the zeroes to 1 and ones to -1. When 0 will be flipped, we will gain a 1. When 1 will be flipped, we will lose 1.

Then the problem changes to finding the maximum sum subarray's start and end position. We can tweak Kadane's Algorithm to do this by tracking the L and R for the maximum sub subarray.

```java
public class Solution {

    public int[] flip(String A) {

        int n = A.length();

        int[] arr = new int[n];

        // Converting the input string into an array of 1s and -1s
        for(int i=0; i<n; i++){
            if(A.charAt(i) == '0'){
                arr[i] = 1;
            } else {
                arr[i] = -1;
            }
        }

        int maxSum = 0;
        int currentSum = 0;
        int L = 0;
        int R = 0;

        int[] ans = new int[]{-1, -1};

        boolean foundOne = false; // Flag to check if there's at least one '1' in the input string

        for(int i=0; i<n; i++){

            if(arr[i] == 1) foundOne = true; // Set the flag to true if encountering a '1'

            currentSum += arr[i]; // Add the current element to the current sum

            if(currentSum > maxSum){

                maxSum = currentSum; // Update the maximum sum found so far

                R = i; // Update the ending index of the potential subarray

                // Update the answer array with the potential subarray indices
                ans[0] = L+1;
                ans[1] = R+1;
            }

            if(currentSum < 0){

                currentSum = 0; // Reset the current sum if it becomes negative

                //IMPORTANT: when the currentSum becomes 0, we need to discard the previous subarray and start checking from the next index

                L = i + 1; // Move the starting index of the potential subarray to the next index

            }

        }

        if(!foundOne){ // If no '1' was found in the input array, return an empty array
            return new int[]{};
        }

        return ans;

    }

}
```
