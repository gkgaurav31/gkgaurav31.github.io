---
layout: post
title: Merge Triplets to Form Target Triplet
date: 2023-01-29 13:55 +0530
author: "Gaurav Kumar"
tags: "java leetcode greedy neetcode150"
categories: "neetcode150"
---

## PROBLEM DESCRIPTION

A triplet is an array of three integers. You are given a 2D integer array triplets, where triplets[i] = [ai, bi, ci] describes the ith triplet. You are also given an integer array target = [x, y, z] that describes the triplet you want to obtain.

To obtain target, you may apply the following operation on triplets any number of times (possibly zero):

> Choose two indices (0-indexed) i and j (i != j) and update triplets[j] to become [max(ai, aj), max(bi, bj), max(ci, cj)].
> For example, if triplets[i] = [2, 5, 3] and triplets[j] = [1, 7, 5], triplets[j] will be updated to [max(2, 1), max(5, 7), max(3, 5)] = [2, 7, 5].

Return true if it is possible to obtain the target triplet [x, y, z] as an element of triplets, or false otherwise.

[leetcode](https://leetcode.com/problems/merge-triplets-to-form-target-triplet/description/)

### SOLUTION

[NeetCode YouTube](https://www.youtube.com/watch?v=kShkQLQZ9K4)

Observation:  

If there is a triplet, which has any element which is more than the element in the target at that position, we cannot use that triplet.
If it's not the case, then that triplet can be used to form the target, if any of the positions match.

```java
class Solution {

    public boolean mergeTriplets(int[][] triplets, int[] target) {
        
        //to store which positions are possible
        Set<Integer> set = new HashSet<>();

        //for each triplet
        for(int i=0; i<triplets.length; i++){
            
            //if any of the numbers in current triplet are more than the corresponding target, we cannot use it so continue to next triplet
            if(triplets[i][0] > target[0] || triplets[i][1] > target[1] || triplets[i][2] > target[2]) continue;

            //otherwise, check if the current triplet has any matching positions with target
            for(int j=0; j<3; j++){

                //if it does have a matching number, store its position in the set
                //we are using a set as there are be many matches with other triplets
                if(triplets[i][j] == target[j]){
                    set.add(j);
                }
            }

        }

        //finally, if we have found matches for all 3 positions, the set size must be 3
        return set.size()==3;

    }

}
```
