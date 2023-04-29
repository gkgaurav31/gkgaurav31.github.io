---
layout: post
title: First Repeating element
date: 2023-04-29 10:15 +0530
author: "Gaurav Kumar"
tags: "java arrays hashmap interviewbit"
categories: "arrays"
---

### PROBLEM DESCRIPTION

Given an integer array A of size N, find the first repeating element in it.
We need to find the element that occurs more than once and whose index of first occurrence is smallest.
If there is no repeating element, return -1.

### SOLUTION

```java
public class Solution {

    public int solve(int[] A) {

        int n = A.length;

        //HashMap to store the index of first occurance of all elements
        Map<Integer, Integer> map = new HashMap<>();

        for(int i=0; i<n; i++){
            if(!map.containsKey(A[i])){
                map.put(A[i], i);
            }
        }

        //init with n or Integer.MAX_VALUE
        int firstOccurance = n;

        //hashset to identify duplicates
        Set<Integer> set = new HashSet<>();

        //iterate all elements one by one
        for(int i=0; i<n; i++){
            
            //if it's repeating
            if(set.contains(A[i])){
                
                //check if its first occurance has a lower value
                if(map.get(A[i]) < firstOccurance){
                    //update the first occurance in that case
                    firstOccurance = map.get(A[i]);
                }

            //else add it to the set so compare later
            }else{
                set.add(A[i]);
            }
        }

        //if there was no repeating element, firstOccurance will still be set to n
        return firstOccurance==n?-1:A[firstOccurance];

    }

}

```
