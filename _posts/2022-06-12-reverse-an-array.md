---
layout: post
title: Reverse an Array
author: "Gaurav Kumar"
tags: "java arrays"
categories: "arrays"

---

## Problem Description:  Given an Array, reverse it

### Solution

If we swap first element with last element, then 2nd element with 2nd last element, 3rd element and 3rd last element and so on, we will finally get a reversed array.

```java
package com.arrays;

import java.util.Arrays;

public class ReverseArray {

	public static void main(String[] args) {
	
		int[] arr = {1,2,3,4,5,6,7,8,9,10};
		
		reverseArray(arr);
		System.out.println(Arrays.toString(arr));
	}
	
	public static int[] reverseArray(int[] arr) {
		
		int i=0,j=arr.length-1;
		
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
