---
layout: post
title: Reverse an Array in the given Range
author: "Gaurav Kumar"
tags: "java"
categories: "arrays"

---

## Problem Description:  Given an Array, reverse it

### Solution

This is similar to how we would reverse an array, but we will need to change the start and end index. Then keep swapping the elements from start and end one by one.

```java
package com.arrays;

import java.util.Arrays;

public class ReverseArray {

	public static void main(String[] args) {
	
		int[] arr = {1,2,3,4,5,6,7,8,9,10};
		
		int start = 3, end = 8;
		reverseArrayWithRange(arr, start, end);
		
		System.out.println(Arrays.toString(arr));
	}
	
	public static int[] reverseArrayWithRange(int[] arr, int start, int end) {
		
		int i=start,j=end;
		
		while(i<j) {
			swap(arr, i, j);
			i++;
			j--;
		}
		
		return arr;
		
	}
	
	public static void swap(int[] arr, int i, int j) {
		int temp = arr[i];
		arr[i] = arr[j];
		arr[j] = temp;
	}
	
	
	
}
```
