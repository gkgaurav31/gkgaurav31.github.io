---
layout: post
title: WoodCutting Made Easy! (InterviewBit)
date: 2024-02-11 16:45 +0530
author: "Gaurav Kumar"
tags: "java binary_search"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

There is given an integer array `A` of size `N` denoting the heights of `N` trees.

Lumberjack Ojas needs to chop down B metres of wood. It is an easy job for him since he has a nifty new woodcutting machine that can take down forests like wildfire. However, Ojas is only allowed to cut a single row of trees.

Ojas's machine works as follows: Ojas sets a height parameter `H` (in metres), and the machine raises a giant sawblade to that height and cuts off all tree parts higher than `H` (of course, trees not higher than `H` meters remain intact). Ojas then takes the parts that were cut off. For example, if the tree row contains trees with heights of 20, 15, 10, and 17 metres, and Ojas raises his sawblade to 15 metres, the remaining tree heights after cutting will be 15, 15, 10, and 15 metres, respectively, while Ojas will take 5 metres off the first tree and 2 metres off the fourth tree (7 metres of wood in total).

Ojas is ecologically minded, so he doesn't want to cut off more wood than necessary. That's why he wants to set his sawblade as high as possible. Help Ojas find the maximum integer height of the sawblade that still allows him to cut off at least `B` metres of wood.

NOTE:

The sum of all heights will exceed `B`, thus Ojas will always be able to obtain the required amount of wood.

## SOLUTION

```java
public class Solution {

    public int solve(int[] A, int B) {

        int n = A.length; // Number of trees

        // Initializing the range for binary search
        long l = 0; // lowest height of blade (entire tree cut)
        long r = A[0]; // height height of blade, which will be max height of tree

        // maximum tree height
        for(int i=0; i<n; i++){
            r = Math.max(r, A[i]);
        }

        // Initializing maximum blade height to min and then try to increase it using binary search
        long maxBladeHeightPossible = 1;

        // Binary search to find the maximum blade height
        while(l<=r){

            // Midpoint of the current range
            long m = (l+r)/2;

            // Checking if it is possible to cut at least B meters of wood with the current blade height
            if(isPossible(A, B, m)){

                // Updating the maximum blade height if possible
                maxBladeHeightPossible = Math.max(maxBladeHeightPossible, m);

                // Search on right for even higher height of the blade if possible
                l = m+1;
            }else{

                // Since current height of blade did not cut needed length of woods, we need to try keep the height lower
                r = m-1;
            }

        }

        return (int) maxBladeHeightPossible;

    }

    // Method to check if it's possible to cut at least 'heightNeeded' meters of wood with 'currentBladeHeight'
    public boolean isPossible(int[] trees, long heightNeeded, long currentBladeHeight){

        // Number of trees
        int n = trees.length;

        // Initializing total length of woods cut
        long totalLengthOfWoodsCut = 0;

        // Calculating the total length of woods cut with the current blade height
        for(int i=0; i<n; i++){

            // If tree height is less than the current blade height, skip
            if(trees[i] < currentBladeHeight) continue;

            // Add the length of woods cut from the current tree
            totalLengthOfWoodsCut += (trees[i] - currentBladeHeight);

        }

        // Return true if total length of woods cut is greater than or equal to 'heightNeeded'
        return totalLengthOfWoodsCut >= heightNeeded;

    }

}
```
