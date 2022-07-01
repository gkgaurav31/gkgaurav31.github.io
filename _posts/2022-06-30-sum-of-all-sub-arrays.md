---
layout: post
title: Sum of all Sub-Arrays
date: 2022-06-30 22:47 +0530
---

### Problem Description

Return the total sum of all sub-arrays of the given array.

### Solution

#### BRUTE-FORCE

The brute force method is to check all sub-array, calculate its sum and add that to the total sum.

```java
public static int totalSumSubArrayBruteForce(ArrayList<Integer> A) {
    
    int totalSum = 0;
    
    for(int i=0; i<A.size(); i++) {
        int currentSum=0;
        for(int j=i; j<A.size(); j++) {
            
            for(int k=i; k<=j; k++) {
                currentSum+=A.get(k);
            }
            
        }
        totalSum+=currentSum;
    }
    
    return totalSum;
    
}
```

#### OPTIMIZATION

We can optimize it to O(N) with a few observations:  

> [1, 2, 3, 4]  
> 0  1  2  3

Let's say, we need to find out __"How many sub-arrays are there, which contain the index "2".__

![snapshot]({{ site.baseurl }}/assets/img/index_in_subarray.jpg)

We can say that the sub-array which contains index 2, must have a starting index in (0, 1, 2). Note that we are including 2 since [2] itself will also be considered as a sub-array with single element.  
Similarly, we can say that the sub-array which contains index 2 must have an end index in (2, 3).

So, total number of sub-arrays which contains index 2 are:  

3 x 2 = 6 (number of indices on left multiplied by number of indices on the right)  

> We can now say that, index 2, which has the element 3 is present in 6 sub-arrays :heavy_check_mark:

This observation is very important because now, we know that the contributation of index 2 (element 3) to the total sum of all sub-arrays is: 3 x 6 = 18.

In general, the number of sub-arrays possible for any index i will be:  

> (i+1) x (N-i) where N is the length of the array

Using this approach, we can calculate the total number of sub-arrays containing index 0,1,2...(n-1).  

The contribution to total sum by each element will be: (element at i index) X (number of sub-arrays containing index i). And the total sum of all sub-arrays will be the total sum of all the previous resulsts, which is the contribution of each element to the total sum.

```java
package com.test;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class Solution {

public static void main(String[] args) {
    List<Integer> list = Arrays.asList(1, 2, 3, 4);
    ArrayList<Integer> l = new ArrayList<>(list);
    System.out.println(totalSumSubArray(l));
    System.out.println(totalSumSubArrayBruteForce(l));
}

public static int totalSumSubArrayBruteForce(ArrayList<Integer> A) {
    
    int totalSum = 0;
    
    for(int i=0; i<A.size(); i++) {
        int currentSum=0;
        for(int j=i; j<A.size(); j++) {
            
            for(int k=i; k<=j; k++) {
                currentSum+=A.get(k);
            }
            
        }
        totalSum+=currentSum;
    }
    
    return totalSum;
    
}

public static int totalSumSubArray(ArrayList<Integer> A) {
    
    int totalSum = 0;
    
    for(int i=0; i<A.size(); i++) {
        totalSum += solve(A, i) * A.get(i);
    }
    
    return totalSum;
    
}

//Given a list, how many subarrays have the given index present in it?
public static int solve(ArrayList<Integer> A, int index) {

    //The starting index can be: [0,1,2,3...index]
    //The end index can be: [index, index+1, index+1...n-1]
    //So, the total possible subarrays will be, total combinations possible for the above values
    
    int start = index+1;
    int end = A.size()-index;
    
    return start*end;
}
}
```
