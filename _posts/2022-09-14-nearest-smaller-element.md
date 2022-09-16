---
layout: post
title: Nearest Smaller Element
date: 2022-09-14 21:14 +0530
author: "Gaurav Kumar"
tags: "java stack interviewbit important"
---

### PROBLEM DESCRIPTION

Given an array, find the nearest smaller element G[i] for every element A[i] in the array such that the element has an index smaller than i.

More formally,

    G[i] for an element A[i] = an element A[j] such that 
    j is maximum possible AND 
    j < i AND
    A[j] < A[i]

Elements for which no smaller element exist, consider next smaller element as -1.

[InterviewBit](https://www.interviewbit.com/problems/nearest-smaller-element/)

### SOLUTION

![snapshot]({{ site.baseurl }}/assets/img/nearest_smaller_element.png)

```java
package com.gauk.stacks;

import java.util.Arrays;
import java.util.Stack;

public class NearestSmallest {

    public static void main(String[] args) {
        int[] a = new int[]{39, 27, 11, 4, 24, 32, 32, 1};
        System.out.println(Arrays.toString(getNearestSmallerOnLeft(a)));
        //Output: [-1, -1, -1, -1, 4, 24, -1, -1]
    }

    public static int[] getNearestSmallerOnLeft(int[] arr){

        int[] out = new int[arr.length];
        Arrays.fill(out, -1);

        Stack<Integer> stack = new Stack<>();
        stack.push(arr[0]);

        for(int i=1; i<arr.length; i++){

            while(!stack.isEmpty() && arr[i] <= stack.peek()){
                stack.pop();
            }

            if(!stack.isEmpty() && arr[i] > stack.peek()){
                out[i] = stack.peek();
            }

            stack.push(arr[i]);

        }

        return out;

    }

}
```
