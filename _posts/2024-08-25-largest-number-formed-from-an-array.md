---
layout: post
title: Largest Number formed from an Array (geeksforgeeks - SDE Sheet)
date: 2024-08-25 15:15 +0530
author: "Gaurav Kumar"
tags: "java strings arrays geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given an array of strings arr[] of length n representing non-negative integers, arrange them in a manner, such that, after concatenating them in order, it results in the largest possible number. Since the result may be very large, return it as a string.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/largest-number-formed-from-an-array1117/1?page=2)

## SOLUTION

The main idea it to use a custom comparator to compare two strings and use that to sort the given array. Once the array is sorted, we just need to concatenate all the strings from left to right, since we sorted in descending order to get the largest value.

```java
class Solution {

    String printLargest(int n, String[] arr) {

        Arrays.sort(arr, new MyComparator());

        StringBuffer sb = new StringBuffer();

        for(int i=0; i<n; i++)
            sb.append(arr[i]);

        return sb.toString();
    }

}

class MyComparator implements Comparator<String>{

    @Override
    public int compare(String a, String b){

        String s1 = a + b;
        String s2 = b + a;

        return s2.compareTo(s1);

    }

}
```
