---
layout: post
title: Minimum Size Subarray Sum
date: 2023-07-29 17:19 +0530
author: "Gaurav Kumar"
tags: "java sliding_window arrays leetcode leetcode150 important"
categories: "arrays"
---

## PROBLEM DESCRIPTION

Given an array of positive integers nums and a positive integer target, return the minimal length of a subarray whose sum is greater than or equal to target. If there is no such subarray, return 0 instead.

[leetcode](https://leetcode.com/problems/minimum-size-subarray-sum/)

## SOLUTION

### USING PREFIX SUM (TLE)

Using two for-loops,calculate the sum of subarray using a prefix sum array.

```java
class Solution {
    
    public int minSubArrayLen(int target, int[] nums) {

        int n = nums.length;
        
        int[] pf = new int[n];
        
        //init: prefix array to store the cumulative sum
        pf[0] = nums[0];
        for(int i=1; i<n; i++){
            pf[i] = pf[i-1] + nums[i];
        }

        int minLength = Integer.MAX_VALUE;

        for(int i=0; i<n; i++){
            for(int j=i; j<n; j++){
                
                //get sum from index [i,j]
                int s = getSum(pf, i, j);

                //check if the sum is >= target
                if(s >= target){
                    int length = j-i+1;
                    minLength = Math.min(minLength, length);
                    break; //no use in increasing j after this because if even if we get an answer, it will have a higher length
                }

            }
        }
        
        return minLength==Integer.MAX_VALUE?0:minLength;

    }


    public  int getSum(int[] pf, int i, int j){
        if(i == 0){
            return pf[j];
        }
        return pf[j] - pf[i-1];
    }

}
```

### USING PREFIX SUM + BINARY SEARCH

```java
class Solution {

    public int minSubArrayLen(int target, int[] nums) {
        
        int n = nums.length;

        int[] pf = new int[n];
        
        //init: prefix array to store the cumulative sum
        pf[0] = nums[0];
        for(int i=1; i<n; i++){
            pf[i] = pf[i-1] + nums[i];
        }

        //init: setting the answer to INT_MAX
        //this will be updated anytime we have a lower value
        int minLength = Integer.MAX_VALUE;
        
        //fix the start of the array and look for end index
        for(int i=0; i<n; i++){
            
            //it is possible that a single element is >= target, in which case we can directly return 1 as the minimum length
            if(nums[i] >= target){
                return 1;
            }
            
            //otherwise, we will need to look for an end index of an array starting at i
            //we can use binary search (all the numbers are +ve)
            //search range will be from (i+1) to last index (n-1)
            int L = i+1;
            int R = n-1;

            //look for the end index while L<=R
            while(L<=R){
                
                //get the mid
                int mid = (L+R)/2;
                
                //get the sum starting from index i to mid using prefix sum array pf
                int rangeSum = getSum(pf, i, mid);

                //if the sum if less than the target, the needed index cannot be between [i+1, mid]
                //so, update the left index to mid+1
                if(rangeSum < target){
                    L = mid + 1;

                //if the sum is >=target, we have a POTENTIAL answer
                //update minLength if it's lower
                //and try to look for better answer by reducing the right index to mid-1
                }else{
                    minLength = Math.min(minLength, mid-i+1);
                    R = mid - 1;
                }

            }

        }

        //if there was no possible answer, minLength will still be INT_MAX
        //we could also do this check at the beginning by checking if the sum of all elements >=target
        return minLength == Integer.MAX_VALUE ? 0 : minLength;
       

    }

    public  int getSum(int[] pf, int i, int j){
        if(i == 0){
            return pf[j];
        }
        return pf[j] - pf[i-1];
    }
}
```

### SLIDING WINDOW

```java
class Solution {
    
    public int minSubArrayLen(int target, int[] nums) {

        int n = nums.length;

        //to keep track of sum from left to ith index
        int sum = 0;

        int ans = Integer.MAX_VALUE;

        //left of the window
        int left = 0;

        //this will act like the end index for the window
        //we will keep moving i towards right
        //whenever the sum is >= target, try reducing the size of window from left side
        for(int i=0; i<n; i++){

            sum += nums[i];

            //till the time current sum is more than target, try reducing the window from left to get better answer with less size
            //keep updating the answer while reducing the window size
            while(sum >= target){
                
                //update the answer
                ans = Math.min(ans, i-left+1);
                
                //reduce the window from left side and update the sum
                sum = sum - nums[left];
                left++;
                
            }

        }

        return ans == Integer.MAX_VALUE ? 0 : ans;

    }

}
```
