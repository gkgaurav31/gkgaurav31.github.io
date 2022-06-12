---
layout: post
title: Rotate an Array k times
author: "Gaurav Kumar"
tags: "java, arrays"
categories: "arrays"

---

## Problem Description:  Given an Array, rotate it k times. 

### Example

__Array:__
1 2 3 4 5 6 7 8 9  
__k: 3__  
__Output:__
7 8 9 1 2 3 4 5 6  

### Solution

Array: 1 2 3 4 5 6 7 8 9  
k: 3

In the output array, we can see that the last k elements will appear first, and then the rest of the elements will appear.
So, step 1: reverse the array to get [9 8 7 6 5 4 3 2 1]
The, use the "reverse an array in the given range code" to reverse the first 3 elements. Do the same for remaining elements. This will give us the final answer.

:exclamation: | Since k can be greater than the array length, we will need to use: __k = k%arr.length__

```java
package com.arrays;

import java.util.Arrays;

public class RotateArray {

	public static void main(String[] args) {

		int[] arr = {1,2,3,4,5,6,7,8,9};
		int k = 3;
		
		rotateArrayKTimes(arr, k);
		
		System.out.println(Arrays.toString(arr));

	}
	
	public static int[] rotateArrayKTimes(int arr[], int k) {
		
        k = k%arr.length;

		reverseArray(arr);
		reverseArrayWithRange(arr, 0, k-1);
		reverseArrayWithRange(arr, k, arr.length-1);
		
		return arr;
		
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
