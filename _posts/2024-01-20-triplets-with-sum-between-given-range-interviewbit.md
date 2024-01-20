---
layout: post
title: Triplets with Sum between given range (InterviewBit)
date: 2024-01-20 21:47 +0530
author: "Gaurav Kumar"
tags: "java interviewbit arrays"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given an array of real numbers greater than zero in form of strings. Find if there exists a triplet `(a,b,c)` such that `1 < a+b+c < 2`. Return `1` for `true` or `0` for `false`.

`O(n)` solution is expected.

**Note:** You can assume the numbers in strings don't overflow the primitive data type and there are no leading zeroes in numbers. Extra memory usage is allowed.

[InterviewBit](https://www.interviewbit.com/problems/triplets-with-sum-between-given-range/)

## SOLUTION

```java
public class Solution {

    public int solve(String[] A) {

        // Get the length of the input array
        int n = A.length;

        // If the array has less than 3 elements, it's not possible to form triplets
        if(n < 3) return 0;

        // Convert the input string array to a double array for numeric comparison
        double[] arr = new double[n];
        for(int i=0; i<n; i++){
            arr[i] = Double.parseDouble(A[i]);
        }

        // Sort the array in ascending order
        Arrays.sort(arr);

        // Initialize two pointers, l and r, representing the leftmost and rightmost elements of the array
        int l=0;
        int r=n-1;

        // Loop until there are at least 3 elements between the pointers
        while(r-l+1 >= 3){

            // Calculate the middle index
            int m = (l+r)/2;

            // Calculate the sum of the current triplet
            double currentSum = arr[l] + arr[m] + arr[r];

            // If the sum is greater than 2.0, move the right pointer to the left
            if(currentSum > 2.0){
                r--;
            }
            // If the sum is less than 1.0, move the left pointer to the right
            else if(currentSum < 1.0){
                l++;
            }
            // If the sum is between 1.0 and 2.0, return 1 (true)
            else{
                return 1;
            }
        }

        // If no triplet is found, return 0 (false)
        return 0;
    }
}
```
