---
layout: post
title: Leaders in an array (geeksforgeeks - SDE Sheet)
date: 2024-04-05 21:01 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks arrays sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given an array `A` of positive integers. Your task is to find the leaders in the array. An element of array is a leader if it is greater than or equal to all the elements to its right side. The rightmost element is always a leader.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/leaders-in-an-array-1587115620/1?page=1)

## SOLUTION

### APPROACH 1

Using suffix array to maintain the maximum elements from right.

```java
class Solution{

    static ArrayList<Integer> leaders(int arr[], int n){

        // Creating an array to store the maximum value from the current index to the end of the array.
        int[] sf = new int[n];

        // Initializing the last element of the max suffix array as the last element of the input array.
        sf[n-1] = arr[n-1];

        // fill the suffix array with maximum values from right to left.
        for(int i=n-2; i>=0; i--)
            sf[i] = Math.max(sf[i+1], arr[i]);

        // Leader elements.
        ArrayList<Integer> res = new ArrayList<>();

        // Check if each element is a leader by comparing it with the maximum value to its right.
        for(int i=0; i<n-1; i++){
            if(arr[i] >= sf[i+1]) // If the current element is greater than or equal to the maximum value to its right.
                res.add(arr[i]); // Add it to the result list as a leader.
        }

        // Adding the rightmost element of the input array, which is always a leader.
        res.add(arr[n-1]);

        return res; // Returning the list of leaders.

    }
}
```

### APPROACH 2

Using carry-forward technique to keep a track of maximum element from right to left.

```java
class Solution{
    // Function to find the leaders in the array.
    static ArrayList<Integer> leaders(int arr[], int n){

        // Variable to keep track of the current maximum element.
        int currentMax = arr[n-1];

        // ArrayList to store the leader elements.
        ArrayList<Integer> res = new ArrayList<>();

        // Looping through the array from right to left.
        for(int i=n-1; i>=0; i--){

            // If the current element is greater than or equal to the current maximum.
            if(arr[i] >= currentMax){

                // Add the current element to the result list.
                res.add(arr[i]);

                // Update the current maximum.
                currentMax = arr[i];

            }

        }

        // Since we added elements from right to left, reverse the list to maintain the original order.
        Collections.reverse(res);

        // Return the list of leaders.
        return res;

    }
}
```
