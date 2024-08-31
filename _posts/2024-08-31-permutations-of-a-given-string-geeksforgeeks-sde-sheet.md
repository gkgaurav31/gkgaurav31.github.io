---
layout: post
title: Permutations of a given string (geeksforgeeks - SDE Sheet)
date: 2024-08-31 18:57 +0530
author: "Gaurav Kumar"
tags: "java strings geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a string S. The task is to print all unique permutations of the given string that may contain dulplicates in lexicographically sorted order.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/permutations-of-a-given-string2041/1?page=4)

## SOLUTION

### BRUTE-FORCE - BACKTRACKING (ACCEPTED)

```java
class Solution {

    public List<String> find_permutation(String s) {

        // Convert the input string to a character array for easy manipulation
        char[] arr = s.toCharArray();

        // Use a helper function to find all permutations and store them in a set to ensure uniqueness
        List<String> list = new ArrayList<String>(findPermutationHelper(arr, 0, new HashSet<>()));

        // Sort the list of permutations lexicographically
        Collections.sort(list);

        // Return the sorted list of unique permutations
        return list;
    }

    // Helper function to recursively find all permutations of the character array
    public Set<String> findPermutationHelper(char[] s, int idx, Set<String> set) {

        // Base case: if the index reaches the end of the array, add the current permutation to the set
        if(idx == s.length) {
            set.add(String.valueOf(s));
            return set;
        }

        // Iterate through the array to generate all possible permutations
        for(int i = idx; i < s.length; i++) {

            // Swap the current element with the element at the current index
            swap(s, i, idx);

            // Recursively call the helper function for the next index
            findPermutationHelper(s, idx + 1, set);

            // Backtrack by swapping the elements back to their original positions
            swap(s, i, idx);
        }

        // Return the set containing all unique permutations
        return set;
    }

    public void swap(char[] arr, int i, int j) {
        char t = arr[i];
        arr[i] = arr[j];
        arr[j] = t;
    }
}
```
