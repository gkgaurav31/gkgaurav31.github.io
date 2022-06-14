---
layout: post
title: Prefix Sum Array
date: 2022-06-14 01:17 +0530
author: "Gaurav Kumar"
tags: "java, arrays, geeksforgeeks"
categories: "arrays"
---

## Problem Description

Find prefix Array Sum of a given array. [Prefix Sum Array](https://www.geeksforgeeks.org/prefix-sum-array-implementation-applications-competitive-programming/)

### Solution

```java
package com.arrays;

import java.util.Arrays;

public class PrefixSum {

	public static void main(String[] args) {
		
		int[] arr = {10, 20, 10, 5, 15};
		System.out.println(Arrays.toString(getPrefixSumArray(arr)));
		
	}
	
	public static int[] getPrefixSumArray(int[] arr) {
		
		int[] prefixArray = new int[arr.length];
		
		prefixArray[0] = arr[0];
		
		for(int i=1; i<prefixArray.length; i++) {
			prefixArray[i] = prefixArray[i-1] + arr[i];
		}
		
		return prefixArray;
	}
	
	
}
```
