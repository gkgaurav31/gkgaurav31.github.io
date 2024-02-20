---
layout: post
title: XOR-ing the Subarrays! (InterviewBit)
date: 2024-02-20 20:25 +0530
tags: "java bit_manipulation important"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given an integer array `A` of size `N`.

You need to find the value obtained by XOR-ing the contiguous subarrays, followed by XOR-ing the values thus obtained. Determine and return this value.

```text
For example, if A = [3, 4, 5] :

Subarray    Operation   Result
3       None            3
4       None            4
5       None            5
3,4   3 XOR 4         7
4,5   4 XOR 5         1
3,4,5    3 XOR 4 XOR 5   2

Now we take the resultant values and XOR them together:

3 ⊕ 4 ⊕ 5 ⊕ 7 ⊕ 1⊕ 2 = 6 we will return 6.
```

## SOLUTION

We can solve this using contribution technique. If we know how many times any given element/index will come up considering all subarray, we can find out its contribution to the anwer. Let us say, there is an index `idx`, for which we want to find out the contribution. How many starting positions are possible such that idx is part of it? It will be `idx+1` since there are `idx+1` elements before or till it. Similary, how many ending positions are possible such that idx is part of it? It will be `n-idx`. This is the number of elements on the right or on that index itself. So the total number of possible combination using the start and end indices are `start*end`. This is the total number of times index `idx` will appear in all the subarrays. We know that `a^a = 0`. So, if the occurance is `even` number of times, the contribution will be `0`. If it is `odd` number of times, it will be `a` itself. We will add that to the answer by using `XOR` in that case.

```java
public class Solution {

    public int solve(int[] A) {

        int n = A.length;
        int total = 0; // Variable to store the final result

        // Iterate through each element of the array
        for(int i=0; i<n; i++){

            // Count how many times the current element appears in all possible subarrays
            int timesPresent = timePresentInAllSubArrays(A, i);

            // If the count is odd, XOR the current element with the total
            // We know that a^a = 0, so if it occurs even number of times, its contribution will be 0
            if(timesPresent % 2 != 0){
                total ^= A[i];
            }

        }

        return total;

    }

    // how many times an element appears in all possible subarrays
    public int timePresentInAllSubArrays(int[] A, int idx){

        int n = A.length;

        // Calculate the starting and ending indices for subarrays containing the element at index 'idx'
        int start = idx + 1;
        int end = n - idx;

        // total number of subarrays containing the element
        return start * end;

    }
}
```
