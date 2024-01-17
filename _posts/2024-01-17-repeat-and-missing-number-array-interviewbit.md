---
layout: post
title: Repeat and Missing Number Array (InterviewBit)
date: 2024-01-17 21:10 +0530
author: "Gaurav Kumar"
tags: "java interviewbit arrays"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

You are given a read only array of n integers from 1 to n.

Each integer appears exactly once except A which appears twice and B which is missing.

Return A and B.

Note: Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

Note that in your output A should precede B.

[InterviewBit](https://www.interviewbit.com/problems/repeat-and-missing-number-array/)

## SOLUTION

### APPROACH 1 - USING HASHMAP

```java
public class Solution {

    public int[] repeatedNumber(final int[] A) {

        int n = A.length;

        // Create a HashMap to store the frequency of each integer in the array
        Map<Integer, Integer> map = new HashMap<>();
        for(int i=0; i<n; i++){
            map.put(A[i], map.getOrDefault(A[i], 0) + 1);
        }

        // Initialize an array to store the two numbers A and B
        int[] ans = new int[]{-1, -1};

        // Iterate through the numbers from 1 to n
        for(int i=1; i<=n; i++){

            // Initialize a variable to store the frequency of the current number
            int freq = 0;

            // Check if the current number is present in the HashMap and get its frequency
            if(map.containsKey(i))
                freq = map.get(i);

            // If the frequency is 0, it means the missing number is found
            if(freq == 0){
                ans[1] = i; // Set the missing number (B)
            }

            // If the frequency is 2, it means the repeated number is found
            if(freq == 2){
                ans[0] = i; // Set the repeated number (A)
            }

            // If both A and B are found, exit the loop
            if(ans[0] != -1 && ans[1] != -1) break;

        }

        return ans;

    }
}
```

### APPROACH 2 - MARKING PRESENT VALUES IN THE ARRAY

```java
public class Solution {

    public int[] repeatedNumber(final int[] A) {

        // Get the length of the array
        int n = A.length;

        // Create a new array to store the elements of the given array
        int[] arr = new int[n];
        for(int i=0; i<n; i++){
            arr[i] = A[i];
        }

        // Variable to store the repeated number
        int repeatedNumber = -1;

        // Iterate through the elements of the array
        for(int i=0; i<n; i++){

            // Take the absolute value of the current element
            int x = Math.abs(A[i]);

            // If the element at position (x-1) is already negative, it means x is the repeated number
            if(A[x-1] < 0){
                repeatedNumber = x;
            }else{
                // Mark the element at position (x-1) as negative to indicate its presence
                A[x-1] = -A[x-1];
            }
        }

        // Variable to store the missing number
        int missingNumber = -1;

        // Iterate through the modified array to find the positive element, indicating the missing number
        // if the numbeer at index i is positive (not marked), that means the number (i+1) was not found in the array
        for(int i=0; i<n; i++){
            if(A[i] > 0){
                missingNumber = i+1; // Set the missing number
                break;
            }
        }

        return new int[]{repeatedNumber, missingNumber};

    }
}
```
