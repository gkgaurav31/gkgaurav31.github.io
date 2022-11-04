---
layout: post
title: Koko Eating Bananas
date: 2022-11-04 22:03 +0530
author: "Gaurav Kumar"
tags: "java binary_search neetcode150"
categories: "neetcode150"
---

This question is part of [NeetCode150 series](https://neetcode.io/practice).  

## Problem Description

Koko loves to eat bananas. There are n piles of bananas, the ith pile has piles[i] bananas. The guards have gone and will come back in h hours.

Koko can decide her bananas-per-hour eating speed of k. Each hour, she chooses some pile of bananas and eats k bananas from that pile. If the pile has less than k bananas, she eats all of them instead and will not eat any more bananas during this hour.

Koko likes to eat slowly but still wants to finish eating all the bananas before the guards return.

Return the minimum integer k such that she can eat all the bananas within h hours.

[leetcode](https://leetcode.com/problems/koko-eating-bananas/description/)

### Solution

```java
class Solution {
    
    public int minEatingSpeed(int[] piles, int h) {

        int l=1; //at least 1 banana per hour

        //the max time can be max banana among all piles
        int r=piles[0]; 
        for(int i=0; i<piles.length; i++){
            r = Math.max(r, piles[i]);
        }

        //let answer be max possible, look for better answer
        int ans = r;

        while(l<=r){
            
            int m = (l+r)/2;

            //If m bananas per hour is possible answer, save and check for lesser 
            if(check(piles, m, h)){
                ans = m;
                r=m-1;
            //Else, check for greater answer since decreasing won't help for sure
            }else{
                l=m+1;
            }

        }

        return ans;
        
    }

    //Check if m banana per hour is possible
    public boolean check(int[] piles, int m, int h){

        int totalTime = 0;
        int idx=0;

        while(idx<piles.length){
            totalTime += (int)Math.ceil(piles[idx]/(double)m);

            //If totalTime is more than allowed time h, return false
            if(totalTime > h) return false;
            idx++;
        }

        return true;

    }


}
```
