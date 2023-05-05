---
layout: post
title: Minimum Swaps required to group all 1s together
date: 2023-05-05 23:59 +0530
author: "Gaurav Kumar"
tags: "java arrays geeksforgeeks"
categories: "arrays"
---

### PROBLEM DESCRIPTION

Given an array of 0s and 1s, we need to write a program to find the minimum number of swaps required to group all 1s present in the array together.

[geeksforgeeks](https://practice.geeksforgeeks.org/problems/minimum-swaps-required-to-group-all-1s-together2451/1?utm_source=gfg&utm_medium=article&utm_campaign=bottom_sticky_on_article)

### SOLUTION

```java
class Complete {

    public static int minSwaps(int arr[], int n) {
        
        //window size will be equal to the number of 1st present in the array
        int window = countOne(arr, n);
        
        //edge case
        if(window == 0) return -1;
        
        //prefix array to get the count of 1s within a given range [L, R]
        int[] pf = new int[n];
        pf[0] = arr[0];
        for(int i=1; i<n; i++){
            pf[i] = pf[i-1] + arr[i];
        }
        
        //answer        
        int minSwap = n;
        
        //loop through all the windows
        for(int i=0; i<=n-window; i++){
            
            //left and right of the window
            int l = i;
            int r = i+window-1;
            
            //count of 1st in that range
            int currentCount = rangeSum(l, r, pf);
            
            //we need (window size - number of 1s present in the current window) swaps to have all 1s in the current window
            int swapNeeded = window - currentCount;
            
            //update answer if it's lesser
            minSwap = Math.min(minSwap, swapNeeded);
            
        }
        
        return minSwap;
        
    }
    
    //get the sum between index [l, r] using prefix sum array pf
    public static int rangeSum(int l, int r, int[] pf){
        if(l == 0)
            return pf[r];
        else
            return pf[r] - pf[l-1];
    }
    
    //count the number of 1s in the array
    public static int countOne(int[] arr, int n){
        
        int c = 0;
            
        for(int i=0; i<n; i++){
            if(arr[i] == 1) c++;
        }
        
        return c;
    }
}
```
