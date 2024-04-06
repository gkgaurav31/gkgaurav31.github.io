---
layout: post
title: Find all pairs with a given sum (geeksforgeeks - SDE Sheet)
date: 2024-04-06 17:14 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks arrays sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given two unsorted arrays `A` of size `N` and `B` of size `M` of distinct elements, the task is to find all pairs from both arrays whose sum is equal to `X`.

**Note:** All pairs should be printed in increasing order of `u`. For eg. for two pairs `(u1,v1)` and `(u2,v2)`, if `u1` < `u2` then
`(u1,v1)` should be printed first else second.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/find-all-pairs-whose-sum-is-x5808/1?page=1)

## SOLUTION

```java
class Solution {

    public pair[] allPairs(long A[], long B[], long N, long M, long X) {

        // Create a HashSet to store elements from array A
        Set<Long> set = new HashSet<>();

        // Add elements from array A to the HashSet
        for(int i=0; i<N; i++)
            set.add(A[i]);

        // Create an ArrayList to store pairs that satisfy the condition
        List<pair> list = new ArrayList<>();

        // Iterate through array B
        for(int i=0; i<M; i++){

            // Check if there exists an element in array A such that their sum equals X
            if(set.contains(X-B[i])){
                // If such an element exists, add the pair (X-B[i], B[i]) to the list
                list.add(new pair(X-B[i], B[i]));
            }
        }

        // Convert the list to an array of pairs
        pair[] res = new pair[list.size()];
        for(int i=0; i<list.size(); i++)
            res[i] = list.get(i);

        // Sort the array of pairs in increasing order of the first element of each pair
        Arrays.sort(res, (a, b) -> (int) a.first - (int) b.first);

        return res;
    }

}
```
