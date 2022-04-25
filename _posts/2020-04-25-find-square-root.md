---
layout: post
title: Find Square Root
author: "Gaurav Kumar"
tags: "java"
categories: "miscelleneous"

---

## Problem Description

Given a number N, find the square root of that number. If the number is not a perfect square, then return -1

## Solution

### First approach - Brute Force

Brute Force: Loop through all the numbers till N and check if there is any number which when multiplied with itself gives N.

### Second approach - Binary Search

Binary Search Approach: Pick up the middle element. Check if it is the required root. If not, check if that number (ixi) is greater than N. If (ixi) is greater than N, it means that we can ignore all the numbers which are right to i.

```java
package com.test;

public class Solution {

	public static void main(String[] args) {
		
		System.out.println(findSquareRootv1(25));
		System.out.println(findSquareRootv2(25));
		
	}
	
	//brute force
	public static int findSquareRootv1(int n) {
		
		for(int i=1; i<=n; i++) {
			if(i*i == n) {
				return i;
			}
		}
		
		return -1;
		
	}
	
	//using binary search approach
	public static int findSquareRootv2(int n) {

		if(n == 0) return 0;
		if(n < 0) return -1;
		
		int[] arr = new int[n];
		
		for(int i=0; i<n;i++) {
			arr[i] = i+1;
		}
		
		int i=0;
		int j=n-1;
		int mid=0;
		
		while(i<=j) {
			
			mid = (i+j)/2;
			int checkNum = arr[mid] * arr[mid];

			if(checkNum == n) {
				return arr[mid];
			}else if(checkNum < n) {
				i = mid+1;
			}else {
				j = mid-1;
			}
			
		}
		
		return -1;
		
	}
	
}

```
