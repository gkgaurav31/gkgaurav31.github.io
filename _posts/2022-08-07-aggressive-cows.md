---
layout: post
title: Aggressive Cows
date: 2022-08-07 01:45 +0530
author: "Gaurav Kumar"
tags: "java arrays search important"
categories: "search"

---

## Problem Description

Given an array of length N, where each element denotes the position of a stall. Now you have N stalls and an integer K which denotes the number of cows that are aggressive. To prevent the cows from hurting each other, you need to assign the cows to the stalls, such that the minimum distance between any two of them is as large as possible. Return the largest minimum distance.  
[LINK](https://leetcode.com/discuss/general-discussion/1302335/aggressive-cows-spoj-fully-explained-c)  
[geeksforgeeks](https://www.geeksforgeeks.org/place-k-elements-such-that-minimum-distance-is-maximized/)

## Solution

```java
public class Solution {
    public int solve(int[] A, int B) {

        Arrays.sort(A); 

        int l=1; //low can also be set to the min distance possible between adjacent cows (more optimized)
        int h=A[A.length-1] - A[0]; //max possible distance is if the cows are on extreme sides

        int maxDistance = 1;

        while(l<=h){
            
            int m = (l+h)/2;

            if(check(A, m, B)){ //at least distance m is possible to separate each cow from each other 
                //save the current answer and look for better answer to maximize distance
                maxDistance = m; 
                l=m+1; 
            }else{ 
                //if we cannot separate cows with at least m distance, and any distance greater than m is also not possible.
                //look for lower values
                h=m-1;
            }

        }

        return maxDistance;

    }

    public boolean check(int[] A, int distance, int cows){

        int c = 1; //place first cow at 0th index
        int lastCow = A[0]; //will be used to find distance between next cow and this cow

        for(int i=1; i<A.length; i++){ //start from i=1 since we have already placed the first cow at 0th index

            if(A[i] - lastCow >= distance){ //if the distance between the next cow, if we place at index i, is more than min dist we are checking for.
                //place the next cow
                lastCow = A[i];
                c++;
                if(c == cows){ //we've placed all the cows
                    return true;
                }
            }

        }

        return false; //could not place all the cows

    }

}
```
