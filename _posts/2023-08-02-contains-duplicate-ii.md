---
layout: post
title: Contains Duplicate II
date: 2023-08-02 19:48 +0530
author: "Gaurav Kumar"
tags: "java hashmap leetcode leetcode150"
categories: "hashmap"
---

## PROBLEM DESCRIPTION

Given an integer array nums and an integer k, return true if there are two distinct indices i and j in the array such that nums[i] == nums[j] and abs(i - j) <= k.

[leetcode](https://leetcode.com/problems/contains-duplicate-ii/)

## SOLUTION

### APPROACH 1

This is the most intuitive one in which we maintain a HashMap of <Integer, ListOfIndices> for each unique integer while iterating. When a match is found, we iterate over the previous indices seen and check if the distance is at most k. Return true in that case.

```java
public boolean containsNearbyDuplicate(int[] nums, int k) {

    // Get the length of the input array
    int n = nums.length;

    // Create a HashMap to store each element as the key and its indices as the value
    Map<Integer, List<Integer>> map = new HashMap<>();

    // Iterate through the input array
    for (int i = 0; i < n; i++) {

        // Get the current element at index i
        int current = nums[i];

        // Check if the current element already exists in the HashMap
        if (map.containsKey(current)) {

            // If it exists, get the list of indices where this element has appeared before
            List<Integer> t = map.get(current);

            // Iterate through the list of indices
            for (int j = 0; j < t.size(); j++) {

                // Get the previous index where the current element appeared
                int idx = t.get(j);

                // Check if the absolute difference between the current and previous indices is less than or equal to k
                if (Math.abs(i - idx) <= k)
                    return true; // If yes, return true as we found a pair satisfying the condition

            }

            // If the loop completes without returning true, then update the list with the current index
            t.add(i); // This will automatically update the map since it's referring to the same list object

        } else {

            // If the current element is not already in the map,
            // create a new list and add the current index to it
            List<Integer> t = new ArrayList<>();
            t.add(i);
            map.put(current, t);

        }

    }

    // If no such pair is found during the iteration, return false
    return false;
}
```

### APPROACH 2 (SMALL OPTIMIZATION)

In the previous code, it is enough to maintain a HashSet of last k elements. If the same number is found in the last k hashset, we can return true.

```java
class Solution {
    
    public boolean containsNearbyDuplicate(int[] nums, int k) {

        int n = nums.length;

        // Create a HashSet to keep track of the unique elements within the sliding window of size k
        Set<Integer> set = new HashSet<>();

        for(int i=0; i<n; i++){

            // Get the current element at index i
            int current = nums[i];

            // If the current element already exists in the set, it means we found a nearby duplicate
            if(set.contains(current)) return true;

            // Add the current element to the set
            set.add(current);

            // If the size of the set exceeds the allowed window size k, remove the oldest element from the set
            if(set.size() > k){
                set.remove(nums[i-k]); 
            }

        }

        return false;
    }

}
```
