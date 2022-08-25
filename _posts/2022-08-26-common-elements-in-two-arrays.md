---
layout: post
title: Common Elements in Two Arrays
date: 2022-08-26 00:10 +0530
author: "Gaurav Kumar"
tags: "java arrays hashing"
categories: "hashing"
---

## Problem Description

Given two integer arrays, A and B of size N and M, respectively. Your task is to find all the common elements in both the array.
Each element in the result should appear as many times as it appears in both arrays.

### Solution

The elements can repeat and we need to compare the frequently of elements also in the two given arrays.

```java
public class CountCommonElements {

    public int[] solve(int[] A, int[] B) {

        //The maximum number of elements common can be min_length of A and B.
        int[] out = new int[Math.min(A.length, B.length)];

        //How many common elements?
        int count = 0;

        //Get frequency of all elements in first array in a HashMap
        Map<Integer, Integer> map = new HashMap<>();
        for(int i=0; i<A.length; i++){
            if(map.containsKey(A[i])){
                map.put(A[i], map.get(A[i])+1);
            }else{
                map.put(A[i], 1);
            }
        }

        //For each element in second array, check if it's present in the HashMap
        for(int i=0; i<B.length; i++){

            //If the element is present in the HashMap, reduce the frequency by 1, increase count and add it to temp array.
            if(map.containsKey(B[i])){

                int f = map.get(B[i]); //Frequency of B[i] in first array
                out[count] = B[i];
                count++;

                //If the frequency was 1, it was the last element in the first array, so remove it now. 
                if(f == 1){
                    map.remove(B[i]);
                }else{
                //If the frequency is more than 1, reduce it by 1.
                    map.put(B[i], (f-1));
                }
            }
        }

        //No common element.
        if(count == 0) return new int[]{};

        //The number of common elements is equal to count
        int[] arr = new int[count];
        for(int i=0; i<count; i++){
            arr[i] = out[i];
        }

        return arr;

    }
}
```
