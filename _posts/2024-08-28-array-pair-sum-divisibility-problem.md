---
layout: post
title: Array Pair Sum Divisibility Problem (geeksforgeeks - SDE Sheet)
date: 2024-08-28 23:17 +0530
author: "Gaurav Kumar"
tags: "java arrays geeksforgeeks sde-sheet important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given an array of integers nums and a number k, write a function that returns true if given array can be divided into pairs such that sum of every pair is divisible by k.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/array-pair-sum-divisibility-problem3257/1?page=3)

## SOLUTION

- If the array length is odd, return `false` because you can't pair all elements.

- Count how many times each remainder appears when dividing by `k`. Use a frequency array to keep track.

  - For remainder `0`, also check for even occurrences for self-pairing.
  - For remainders equal to `k/2`, make sure there are an even number of them since they pair with themselves.
  - For other remainders, ensure that the count of each remainder matches the count of `k - remainder` for proper pairing.

```java
class Solution {

    public boolean canPair(int[] nums, int k) {

        int n = nums.length;

        // if the length is odd, one num will be left out
        if(n%2 == 1)
            return false;

        // hashmap to store the frequency of remainders possible from nums array
        Map<Integer, Integer> map = new HashMap<>();

        // store the frequency of the remainders
        for(int i=0; i<n; i++){
            map.put(nums[i]%k, map.getOrDefault(nums[i]%k, 0) + 1);
        }

        // iterate over the array
        for(int i=0; i<n; i++){

            // remainder of currentElement%k
            int remainder = nums[i]%k;

            // if the current remainder is 0
            // the frequency corresponding to 0 must be even
            // because it needs to form pair amongst other numbers with 0 remainder
            if(remainder == 0){

                if(map.get(remainder)%2 != 0)
                    return false;

            // if the current remainder is half of k
            // example: 5%10 = 5
            // in this case also, freq of remainder should be even since it will form pair with nums of same remainder
            }else if(k%2 == 0 && remainder == k/2){

                if(map.get(remainder)%2 != 0)
                    return false;
            // otherwise, we need to form pair between nums with remainder (x) and (k-x)
            // so the frequency of numbers with remainder x and k-x must be same
            }else{

                if(map.get(remainder) != map.get(k - remainder))
                    return false;

            }

        }

        return true;

    }

}
```
