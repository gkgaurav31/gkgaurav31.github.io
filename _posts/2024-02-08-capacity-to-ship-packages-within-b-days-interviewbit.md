---
layout: post
title: Capacity To Ship Packages Within B Days (InterviewBit)
date: 2024-02-08 20:51 +0530
author: "Gaurav Kumar"
tags: "java binary_search"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

A conveyor belt has packages that must be shipped from one port to another within `B` days.

The ith package on the conveyor belt has a weight of `A[i]`. Each day, we load the ship with packages on the conveyor belt (in the order given by weights). We may not load more weight than the maximum weight capacity of the ship.

Return the least weight capacity of the ship that will result in all the packages on the conveyor belt being shipped within `B` days.

## SOLUTION

The minimum weight we will definitely need to keep will be the maximum weight amongst all the items. Also, if we have allowedWeight equal to the sum of all the weights of all items, we can deliver it in just 1 day. So, that is the maximum weight possible to deliver the items within B days. But we need to optimize this value to something lower, which would still allow us to deliver the items within B days. So our range is `[maxWeightAmongAllItems, sumOfAllWeights]`. We can use binary search to look for the best value within this range. We pick up a middle value as usual. Then we check if that middle value as weight would be possible for our answer. Check if we have weightCapacity as the mid value, is it possible to deliver the items in B days? If yes, that can be a possible answer. However, there can be a lower value so we just record it and look for even lower values by reducing our range. If that capacity is not possible, we know any lower capacity will not work. So, we look for only higher value and reduce our search range in binary search.

```java
public class Solution {

    public int solve(int[] A, int B) {

        int n = A.length;

        // Calculate the total weight of all packages
        long totalWeight = totalWeight(A);

        // Number of days allowed for shipping
        long daysAllowed = B;

        // The minimum weight needed has to be the weight of the largest item in the array A, otherwise we cannot deliver that item
        // If the capacity was the sum of all the weight, we can deliver it in just 1 day. So that is the largest possible answer
        long l = maxWeight(A);
        long r = totalWeight;

        // Initially set the minimum weight needed to the total weight of all packages. This means, number of days needed can be 1. We will try to look for a weight within the range [l,r] which can still allow us to deliver the items within B days.
        long minWeightNeeded = totalWeight;

        // Binary search for the minimum weight capacity needed
        while(l <= r){

            long m = (l + r) / 2; // Calculate the middle point of the range

            // Check if it's possible to ship all packages within B days using the current weight capacity m
            if(isPossible(A, m, B)){

                // If possible, update the minimum weight needed and move towards lower weights (left side) to look for lower weight possible
                minWeightNeeded = Math.min(minWeightNeeded, m);
                r = m - 1;

            } else {

                // If not possible, move towards higher weights (right side)
                l = m + 1;

            }
        }

        return (int) minWeightNeeded;
    }

    // Method to find the maximum weight among all packages
    public long maxWeight(int[] A){
        int max = A[0];
        for(Integer x: A)
            max = Math.max(max, x);

        return max;
    }

    // Method to calculate the total weight of all packages
    public long totalWeight(int[] A){
        long sum = 0;
        for(Integer x: A)
            sum += x;

        return sum;
    }

    // Method to check if it's possible to ship all packages within B days using a given weight capacity
    public boolean isPossible(int[] A, long weightAllowed, long daysAllowed){
        long n = A.length;

        long currentWeight = 0;
        long daysNeeded = 0;

        for(int i = 0; i < n; i++){

            // If adding the weight of the current package doesn't exceed the weight capacity
            // add it to the currentWeight being carried
            if(currentWeight + A[i] <= weightAllowed){

                currentWeight += A[i];

            } else {

                // If adding the weight of the current package exceeds the weight capacity
                // increment daysNeeded and start carrying the current package in a new batch
                daysNeeded++;
                currentWeight = A[i];

            }

            // If at any point we need more days and allowed, we can return false (small optimization)
            if(daysNeeded > daysAllowed) return false;

        }

        // If there's any remaining weight, it means there's an incomplete batch, so increment daysNeeded
        if(currentWeight > 0)
            daysNeeded++;

        // Check if the total days needed for shipping is less than or equal to the allowed days
        return daysNeeded <= daysAllowed;

    }
}
```
