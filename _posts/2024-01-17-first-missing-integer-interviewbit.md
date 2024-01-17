---
layout: post
title: First Missing Integer (InterviewBit)
date: 2024-01-17 20:12 +0530
author: "Gaurav Kumar"
tags: "java interviewbit arrays"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given an unsorted integer array, find the first missing positive integer.
Your algorithm should run in O(n) time and use constant space.

[InterviewBit](https://www.interviewbit.com/problems/first-missing-integer/)

## SOLUTION

Let's understand the solution with the help of an example:

> A = [3 4 -1 1]

We need to first for the first positive integer which is missing in this array. This array is of size 4, so the numbers expected to be present are -> 1, 2, 3 and 4. The first number missing amongst them is our answer. It's also possible that all 4 numbers are present, for example `A = [1 2 3 4]`. In that case, the first missing positive number will be 5, since that is the next +ve number not present in the array.

First, let us get rid of the -ve values. You will soon understand why we are doing this. The negatives values cannot be our answer. So we can set it to some +ve value which should cannot be the answer. It's okay to set it to +INFINITY also. In our case, we will just set it to `size_of_array+1 = 5`.

So, now the array will be:

> A = [3 4 5 1]

The size of the array is 4. The expected first four +ve numbers that we are looking for are: `[1, 4]`.

The problem also expects us to not use any extra space. So, we need a way to mark a value if it's present using the existing array itself!

Let us say, if value X is found, we will mark the index `X` as -ve? Well, that will mostly work, except for the last value which is 4 in this since that will be out of bounds for the array.

We can handle this in a simple way - If value `X` is found and it is in the valid range `[1,4]`, we will mark the index `(X-1)`. For example, if value 3 is found, then we will mark the index (3-1) = 2 index as -ve.

Let us do that:

> A = [3 4 5 1]  
> i=0 -> element 3 -> yes, it's a valid answer, so mark (3-1)=2 index as -ve  
> A = [3 4 -5 1]  
> i=1 -> element 4 -> yes, it's a valid answer, so mark (4-1)=3 index as -ve  
> A = [3 4 -5 -1]  
> i=2 -> element 5 (absolute value) -> no, not in the valid answer range [1,4]  
> A = [3 4 -5 -1]  
> i=3 -> element 1, so mark (1-1)=0 index as -ve  
> A = [-3 4 -5 -1]

We have all the information to find out the missing number now!

> If A[i] is -ve, that means (i+1) was found in the array before.

So, we iterate from `i=0` to `i=n-1` and get the first index which has a positive value (not marked). Let's say that index is `X`. That means, the value `X+1` was not present in the array, otherwise `A[X]` would have been -ve. We can return `X+1` as the answer. If all values from `A[0]` to `A[n-1]` are -ve, we found all numbers in the range `[1,n]`. So the first missing +ve number is `n+1`.

```java
public class Solution {

    public int firstMissingPositive(int[] A) {

        int n = A.length;

        // the valid answer can be in the range [1,n]
        // if all are present, then the first missing number will be n+1
        int validStart = 1;
        int validEnd = n;

        // change -ve to out of range
        for(int i=0; i<n; i++){
            if(A[i] <= 0) A[i] = n+1;
        }

        // when a valid x is found, mark index (x-1) -ve
        // by doing this, we know that if the value at any index i is -ve, then i+1 must be present in the array
        for(int i=0; i<n; i++){

            int currentVal = Math.abs(A[i]); // the value may have been already set to -ve earlier, so take absolute first

            //check if the value is in the valid range
            if(currentVal >= validStart && currentVal <= validEnd){
                // there can be duplicates in the array, so make it -ve only if it was +ve earlier
                if(A[currentVal-1] > 0)
                    A[currentVal-1] = -A[currentVal-1];
            }

        }

        // if any of the value as a +ve value (not marked with -ve earlier), we can say that (i+1) is the first missing number
        for(int i=0; i<n; i++){
            if(A[i] > 0){
                return i+1;
            }
        }

        // all numbers from 1 to n were found, so the next missing has to be n+1;
        return n+1;

    }

}
```
