---
layout: post
title: Find Leader in an Array (Carry Forward Technique)
date: 2022-06-14 23:35 +0530
uthor: "Gaurav Kumar"
tags: "java arrays geeksforgeeks"
categories: "arrays"
---

## Problem Description

Given an array, find the numbers of leaders in the array. A leader is an element which is strictly greater than all the elements on its right.
We can consider the last element to be the leader.  

[Leaders in an array](https://www.geeksforgeeks.org/leaders-in-an-array/)

### Solution

The idea is to keep a track of the largest number of right to left. If the next element from left in the loop is greater than the currentMax, we can increase the total count by one.

```java
package com.arrays;

public class FindLeader {

	public static void main(String[] args) {
		System.out.println(findLeaders(new int[] {15,-1, 7,2,5,4,2,3}));
	}
	
	public static int findLeaders(int[] arr) {
		
		int total=1;
		int currentMax=arr[arr.length-1];
		
		for(int i=arr.length-2; i>=0; i--) {
			
			if(arr[i] > currentMax) {
				total++;
				currentMax = arr[i];
			}
			
		}
		
		return total;
		
	}
	
}

```
