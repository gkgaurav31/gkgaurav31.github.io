---
layout: post
title: Rotate Matrix (InterviewBit)
date: 2024-01-21 22:35 +0530
author: "Gaurav Kumar"
tags: "java interviewbit arrays"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

You are given a `N x N` 2D matrix A representing an image.

Rotate the image by 90 degrees (clockwise).

You need to do this `in place`. Update the given matrix `A`.

[InterviewBit](https://www.interviewbit.com/problems/rotate-matrix/)

## SOLUTION

This problem is same as:
[Rotate Image]({% post_url 2023-02-12-rotate-image %})

This problem can be solved by taking the transpose of the given matrix and then reversing each row one by one (reflection of the matrix).

```java
public class Solution {

    public void rotate(ArrayList<ArrayList<Integer>> a) {

        int n = a.size();

        // transpose
        for(int i=0; i<n; i++){
            for(int j=i; j<n; j++){
                swap(a, i,j);
            }
        }

        // reverse row wise
        for(int i=0; i<n; i++){
            Collections.reverse(a.get(i));
        }

    }

    public void swap(ArrayList<ArrayList<Integer>> a, int i, int j){

        int temp = a.get(i).get(j);
        a.get(i).set(j, a.get(j).get(i));
        a.get(j).set(i, temp);

    }

}
```
