---
layout: post
title: Maximum Consecutive Gap (InterviewBit)
date: 2024-01-21 14:09 +0530
author: "Gaurav Kumar"
tags: "java interviewbit bucketing arrays important"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given an unsorted array, find the maximum difference between the successive elements in its sorted form.
Return 0 if the array contains less than 2 elements.

-You may assume that all the elements in the array are non-negative integers and fit in the 32-bit signed integer range.

- You may also assume that the difference will not overflow.

Try to solve it in linear time/space

[InterviewBit](https://www.interviewbit.com/problems/maximum-consecutive-gap/)

## SOLUTION

### APPROACH 1 - SORTING

This works but it has `O(nlogn)` time complexity since we sorted the array.

```java
public class Solution {

    public int maximumGap(final int[] A) {

        int n = A.length;

        if(n < 2) return 0;

        Arrays.sort(A);
        int maxDiff = 0;

        for(int i=1; i<n; i++){
            maxDiff = Math.max(maxDiff, A[i] - A[i-1]);
        }

        return maxDiff;

    }

}
```

### APPROACH 2 - BUCKETING (PIGEONHOLE PRINCIPLE)

[Pigeonhold Principle - Wikipedia](https://en.wikipedia.org/wiki/Pigeonhole_principle)

[Good Explanation - Prakash Shukla on YouTube](https://www.youtube.com/watch?v=VT-6zwGKYwA)

```java
public class Solution {

    public int maximumGap(final int[] A) {

        int n = A.length;

        if(n < 2) return 0;

        // get the max and min from the array
        int max = Arrays.stream(A).max().orElse(Integer.MIN_VALUE);
        int min = Arrays.stream(A).min().orElse(Integer.MAX_VALUE);

        // edge case
        // if max and min are same, average/max gap will be 0
        if(max == min) return 0;

        // diff between max and min divided by numbers of gaps
        int averageGap = (int)Math.ceil(((double)max-min)/(n-1));

        // for each "bucket", what is the min and max for that bucket?
        int[] minBucket = new int[n];
        int[] maxBucket = new int[n];

        // init
        Arrays.fill(minBucket, Integer.MAX_VALUE);
        Arrays.fill(maxBucket, -1);

        // for any element, the bucket it belongs to is: (A[i]-min)/averageGap
        // update if is the max / min for that bucket
        for(int i=0; i<n; i++){

            int idx = (A[i]-min)/averageGap;

            minBucket[idx] = Math.min(minBucket[idx], A[i]);
            maxBucket[idx] = Math.max(maxBucket[idx], A[i]);

        }

        // to find the gap, we need the max from previous bucket and min from current bucket
        // there is nothing before first bucket, so we can start from i=1 (2nd bucket)
        int prev = maxBucket[0];
        int maxGap = 0;

        for(int i=1; i<n; i++){

            // the bucket can be empty if no value had mapped to that bucket, so skip that
            if(maxBucket[i] == -1) continue;

            // update the maxGap is the diff between min of current bucket and max of previous bucket is larger
            maxGap = Math.max(maxGap, minBucket[i] - prev);

            // update prev as maxBucket[i] so that it can be used for next iteration
            prev = maxBucket[i];

        }

        return maxGap;

    }


}
```
