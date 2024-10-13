---
layout: post
title: Minimum XOR value pair (geeksforgeeks - SDE Sheet)
date: 2024-10-13 17:42 +0530
author: "Gaurav Kumar"
tags: "java trie geeksforgeeks sde-sheet important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given an array of integers of size N find minimum xor of any 2 elements.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/minimum-xor-value-pair/1?page=9)

## SOLUTION

### APPROACH 1

The idea is to sort the array and then find the xor of all the number iteratively and calculate the minimum at every iteration.

XOR between two numbers is smaller when their binary representations are more similar. Numbers that are numerically close are also more likely to have similar binary forms, especially in the most significant bits.

```java
class Solution{

    static int minxorpair(int N, int arr[]){

        Arrays.sort(arr);

        int min = Integer.MAX_VALUE;

        for(int i=1; i<N; i++){
            min = Math.min(min, arr[i]^arr[i-1]);
        }

        return min;

    }

}
```

### APPROACH 2 (USING TRIE)

The main idea is in the `findMinXOR` method. When we add a new number from the array, our goal is to minimize the XOR value with the numbers we've already added to the Trie.

To do this, we want to match the bits in the number we're checking, starting from the most significant bit (MSB). The MSB is important because it has the biggest impact on the XOR result.

- We look at each bit from the highest position to the lowest (least significant bit, or LSB).
  - If the same bit is in the Trie, we follow that path. This is good because it helps keep the XOR value low, contributing `0` at that bit position.
  - If the same bit is not there, we have to go with the opposite bit. This will increase the XOR value because it contributes `1` at that position.

```java
class Solution {

    // number of bits in INT
    private static final int INT_SIZE = 32;

    static int minxorpair(int N, int arr[]) {

        // Create the root of the Trie
        TrieNode root = new TrieNode();

        // Insert the first number into the Trie
        // We will add other numbers one by one
        // Before add that number, we will try to compute the minXOR value possible using the previously added numbers in trie
        insert(root, arr[0]);

        // Initialize minimum XOR to a large value
        int minXOR = Integer.MAX_VALUE;

        // Process each number in the array starting from the second number
        for (int i = 1; i < N; i++) {

            // Find the minimum XOR of arr[i] with numbers already in the Trie
            minXOR = Math.min(minXOR, findMinXOR(root, arr[i]));

            // Insert the current number into the Trie
            insert(root, arr[i]);

        }

        return minXOR;

    }

    static int findMinXOR(TrieNode root, int n) {

        // we will add to this minXOR when we can not find same bit at position i
        int minXOR = 0;

        // Traverse each bit of the number from the most significant bit to the least significant bit
        for (int i = INT_SIZE - 1; i >= 0; i--) {

            // Extract the ith bit
            int bit = ((n >> i) & 1);

            // We want to minimize XOR, so we prefer the same bit if it exists
            if (root.children[bit] != null) {

                // since same bit exist, the value to be added is 0 at this position
                // we can simply go to the next position/bit
                root = root.children[bit];

            } else {

                // If same bit doesn't exist, add this bit to minXOR (XOR contribution)
                minXOR |= (1 << i);

                // Follow the path for the opposite bit
                root = root.children[1 - bit];

            }
        }

        return minXOR;

    }

    static void insert(TrieNode root, int n) {

        // Traverse each bit of the number from the most significant bit to the least significant bit
        for (int i = INT_SIZE - 1; i >= 0; i--) {

            // Extract the ith bit
            int bit = ((n >> i) & 1);

            // Create a new TrieNode if the path for this bit doesn't exist
            if (root.children[bit] == null)
                root.children[bit] = new TrieNode();

            // Move to the next node in the Trie
            root = root.children[bit];

        }
    }
}

class TrieNode {

    // Each node can have two children: 0 or 1 (representing binary digits)
    TrieNode[] children = new TrieNode[2];
}
```
