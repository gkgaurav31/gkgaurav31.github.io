---
layout: post
title: Maximum Sum Triplet (InterviewBit)
date: 2024-01-06 13:57 +0530
Author: "Gaurav Kumar"
tags: "java arrays greedy interviewbit important"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given an array A containing N integers.

You need to find the maximum sum of triplet `( Ai + Aj + Ak )` such that `0 <= i < j < k < N` and `Ai < Aj < Ak`.

If no such triplet exist return 0.

[interviewbit](https://www.interviewbit.com/problems/maximum-sum-triplet/)

## SOLUTION

This problem is quite tricky. Let us try to understand the intuition behind it.

We need to select three numbers from the array such that a < b < c. Amongst all such triplets, we need to find the one with maximum sum and return that sum.

Another way to think about it is:

For any position, we need a number before it which is closest to it AND the largest number after it with the largest value.

Finding the maximum number after a given position can be done simply by using a suffix array based on the maximum value. We can create it in this way:

Start from right to left. If the current value is larger, use that to update the suffix array `sf[]` otherwise keep the previous highest value.

```java
sf[n-1] = A[n-1];

for(int i=n-2; i>=0; i--){
    sf[i] = Math.max(A[i], sf[i+1]);
}
```

Finding the largest value before the current position which is still less than the current value needs a bit more thought.

If we had a sorted list, and wanted to find such a number then we could have used binary search. Let's say, the mid is m.

If `A[m] < A[secondNumberIndex]`, which means we got some number which is less than the second number, we have found a potential first number. Potential because there can be another number which is higher than A[m] but still less than A[secondNumberIndex]. We should easily say that we should look for that number after the current `m` index. If that is not the case, look before `m` index.

To make the above simpler, we can use TreeSet in Java that will help us in maintaining the sorted list of numbers before the current position. Then, to find the first number which is closest to second number but strictly less than that, we can use the `lower()` method.

[TreeSet lower()](<https://docs.oracle.com/javase%2F7%2Fdocs%2Fapi%2F%2F/java/util/TreeSet.html#lower(E)>)

Here's how the complete code will look like:

```java
public class Solution {

    public int solve(int[] A) {

        int n = A.length;

        //suffix array to get the max number on right of index i
        int[] maxSuffArray = getMaxSuffixArrayFromRight(A);

        //init: if there are no possible triplets a < b < c, we need to return 0
        int ans = 0;

        //we will maintain a sorted list of numbers from left
        //TreeSet lower() -> Returns the greatest element in this set strictly less than the given element, or null if there is no such element.
        TreeSet<Integer> lowValue = new TreeSet<>();
        lowValue.add(Integer.MIN_VALUE); //to avoid null value if there is no number which is smaller than it

        //we can start the iteration from i=1 also, but we then we would need to add A[0] to the TreeSet
        for(int i=0; i<n-1; i++){

            //the first number will be the closest number to A[i] but strictly less than it
            int num1 = lowValue.lower(A[i]);

            //fix current number as the middle number
            int num2 = A[i];

            //we can get the maximum number on the right of ith index using suffix max array
            int num3 = maxSuffArray[i+1];

            //check if these satify the condition and update the answer
            if(num1 < num2 && num2 < num3){
                ans = Math.max(ans, num1 + num2 + num3);
            }

            //include the current number in the TreeSet for next iteration
            lowValue.add(A[i]);

        }

        return ans;

    }

    //method to create the max sum suffix array
    public int[] getMaxSuffixArrayFromRight(int[] A){

        int n = A.length;
        int[] sf = new int[n];

        sf[n-1] = A[n-1];

        for(int i=n-2; i>=0; i--){
            sf[i] = Math.max(A[i], sf[i+1]);
        }

        return sf;

    }


}
```
