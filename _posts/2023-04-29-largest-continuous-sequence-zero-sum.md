---
layout: post
title: Largest Continuous Sequence Zero Sum
date: 2023-04-29 12:02 +0530
author: "Gaurav Kumar"
tags: "java hashmap interviewbit important"
categories: "hashmap"
---

### PROBLEM DESCRIPTION

Given an array A of N integers.
Find the largest continuous sequence in a array which sums to zero.

[interviewbit](https://www.interviewbit.com/problems/largest-continuous-sequence-zero-sum/)

### SOLUTION

Related Post: [Sub-Array with 0 Sum]({% post_url 2022-06-14-subarray-with-0-sum %})

```java
public class Solution {
    
    public int[] lszero(int[] A) {

        int n = A.length;
        int[] pf = new int[n];

        pf[0] = A[0];

        for(int i=1; i<n; i++){
            pf[i] = pf[i-1] + A[i];
        }

        //first occurance for pf
        Map<Integer, Integer> map = new HashMap<>();

        for(int i=0; i<n; i++){
            if(!map.containsKey(pf[i])){
                map.put(pf[i], i);
            }
        }

        //hashset to check duplicates
        Set<Integer> set = new HashSet<>();

        //init
        int startIndex=-1;
        int maxLength=0;

        //for each element
        for(int i=0; i<n; i++){
            
            //if it is repeating in the pf array => subarray sum from [firstOccurance+1, currentPosition] must be 0
            if(set.contains(pf[i])){
                
                //first occurance of the element
                int first = map.get(pf[i]);

                //current length, starting from first occurance + 1 (Example: A[] => 1 3 -3 :: pf[] => 1 4 1)
                int length = i - (first+1) + 1;

                //if it's more than the maxLength, update the startIndex and length
                if(length > maxLength){
                    startIndex = first+1;
                    maxLength = length;
                }

            //else add to the hashset to compare later
            }else{
                set.add(pf[i]);
            }

            //if the current element itself is 0, that means sub of all elements from index 0 till current position is 0
            //check its length and update max if it's greater than the maxLength
            if(pf[i] == 0 && i+1 > maxLength){
                startIndex = 0;
                maxLength = i+1;
            }

        }

        //final answer
        int[] ans = new int[maxLength];
        for(int i=0; i<ans.length; i++){
            ans[i] = A[startIndex+i];
        }

        return ans;

    }
}

```
