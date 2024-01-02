---
layout: post
title: Occurence of Each Number (InterviewBit)
date: 2024-01-02 23:06 +0530
uthor: "Gaurav Kumar"
tags: "java arrays interviewbit"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

You are given an integer array A.
You have to find the number of occurences of each number.
Return an array containing only the occurences with the smallest value's occurence first.

[interviewbit](https://www.interviewbit.com/problems/occurence-of-each-number/)

## SOLUTION

### APPRAOCH 1 (HashMap)

```java
import java.util.*;

public class Solution {

    public int[] findOccurrences(int[] A) {

        // Sort the array in ascending order
        // We are doing this because the answer needs to be returned based on small number to larger number in arr A
        Arrays.sort(A);

        int n = A.length;

        // Create a HashMap to store elements and their occurrences
        Map<Integer, Integer> map = new HashMap<>();

        // Count occurrences of each element in the array
        for (int i = 0; i < n; i++) {
            map.put(A[i], map.getOrDefault(A[i], 0) + 1);
        }

        // Create an array to store the occurrences
        // The number of unique occurances will be equal to the size of hashmap
        int[] res = new int[map.size()];

        int idx = 0; // Index to fill the result array

        for (int i = 0; i < n; i++) {

            // Skip if the element is a duplicate by comparing it with the previous element
            if (i > 0 && A[i] == A[i - 1]) continue;

            // Store the count of occurrences for each unique element
            res[idx++] = map.get(A[i]);
        }

        return res;

    }

}
```

### APPROACH 2 (TreeMap)

We can simplify our code a bit by making use of TreeMap which will sort the collection based on the Integer Key in our code.

```java
public class Solution {

    public int[] findOccurences(int[] A) {

        Arrays.sort(A);

        int n = A.length;

        //replaced with TreeMap instead of HashMap
        Map<Integer, Integer> map = new TreeMap<>();

        for(int i=0; i<n; i++){
            map.put(A[i], map.getOrDefault(A[i], 0) + 1);
        }

        int[] res = new int[map.size()];

        int idx = 0;

        //The values will be sorted based on Keys already
        //So, we can directly iterated over the values and use them as answer
        for(Integer x: map.values()){
            res[idx++] = x;
        }

        return res;

    }

}
```
