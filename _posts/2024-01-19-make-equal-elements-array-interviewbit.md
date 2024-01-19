---
layout: post
title: Make equal elements Array (InterviewBit)
date: 2024-01-19 21:44 +0530
author: "Gaurav Kumar"
tags: "java interviewbit arrays"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given an array of all positive integers and an element x.

You need to find out whether all array elements can be made equal or not by performing any of the 3 operations:

- add x to any element in array
- subtract x from any element from array
- do nothing.

This operation can be performed only once on an element of array.

[InterviewBit](https://www.interviewbit.com/problems/make-equal-elements-array/)

## SOLUTION

### APPROACH 1 - TRY WITH ALL THREE POSSIBLE VALUES

```java
public class Solution {

    public int solve(int[] A, int B) {

        int n = A.length;

        // If there only 3 possibilities for the values of the final array
        // 1. All of them are equal to A[0]
        // 2. All of them are equal to A[0] - B
        // 3. All of them are equal to A[0] + B
        int[] possibleValues = new int[]{A[0] - B, A[0], A[0] + B};

        // Iterate over the possible values
        for(int i = 0; i < possibleValues.length; i++){

            // Check if all elements in the array can be made equal to the current possible value using B
            if(checkIfAllPossible(A, possibleValues[i], B)){
                // If true, return 1 to indicate that it's possible
                return 1;
            }

        }

        // If no possible value is found, return 0 to indicate that it's not possible
        return 0;

    }

    // The checkIfAllPossible method checks if all elements in the array can be made equal to the given value
    public boolean checkIfAllPossible(int[] A, int value, int B){

        // Iterate over each element in the array A
        for(int i = 0; i < A.length; i++){

            // Check if the current element, or the element after adding or subtracting B, is equal to the given value
            if(A[i] == value || A[i] + B == value || A[i] - B == value)
                // If true, continue to the next iteration
                continue;
            else
                // If false, return false, indicating that not all elements can be made equal to the given value
                return false;
        }

        // all elements can be made equal to the given value, return true
        return true;

    }

}
```

### APPROACH 2 - BUCKET SIZE FOR POSSIBILITIES

For each position i, we have three possible values:

- `A[i]`
- `A[i] + B`
- `A[i] - B`

We can use the above values as keys of a HashMap. We will calculate the above values for each position, and increase the count/value of the corresponding key. If there is a valid solution, at least one of the possible values in the HashMap will become equal to the size of the array.

Example:

> A=[2,3,1]
> B=1

i=0 : Keys -> 2, 1, 3  
i=1 : Keys -> 3, 2, 4  
i=2 : Keys -> 1, 0, 2

HashMap will become ->

> {0:1, 1:2, 2:3, 3:2, 4:1}

This indicates that there are 3 positions which can become value 2. This is equal to size of the array, which means all the positions can be made 2. So, we have a valid answer.

```java
public class Solution {

    public int solve(int[] A, int B) {

        int n = A.length;

        // update frequency for each possibility
        // the three possibilities for each position are:
        // A[i], A[i]+B and A[i]-B
        Map<Integer, Integer> bucket = new HashMap<>();

        for(int i=0; i<n; i++){

            int x = A[i];

            bucket.put(x, bucket.getOrDefault(x, 0)+1);
            bucket.put(x+B, bucket.getOrDefault(x+B, 0)+1);
            bucket.put(x-B, bucket.getOrDefault(x-B, 0)+1);

        }

        // if the count/frequency of any of the possibilities is equal to size of the array, we have a valid answer
        for(Integer value: bucket.values()){
            if(value == n){
                return 1;
            }
        }

        return 0;

    }

}
```
