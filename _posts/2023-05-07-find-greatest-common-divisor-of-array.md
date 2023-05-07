---
layout: post
title: Find Greatest Common Divisor of Array
date: 2023-05-07 16:29 +0530
author: "Gaurav Kumar"
tags: "java modulo gcd leetcode"
categories: "gcd"
---

### PROBLEM DESCRIPTION

Given an integer array nums, return the greatest common divisor of the smallest number and largest number in nums.

The greatest common divisor of two numbers is the largest positive integer that evenly divides both numbers.

[leetcode](https://leetcode.com/problems/find-greatest-common-divisor-of-array/description/)

### SOLUTION

[GCD]({% post_url 2023-05-07-gcd-and-inverse-modulo-theory %})

```java
class Solution {
    
    public int findGCD(int[] nums) {

        int n = nums.length;

        int min = Integer.MAX_VALUE;
        int max = Integer.MIN_VALUE;

        for(int i=0; i<n; i++){
            max = Math.max(max, nums[i]);
            min = Math.min(min, nums[i]);
        }

        return GCD(max, min);

    }

    public int GCD(int a, int b){
        if(b == 0) return a;
        return GCD(b, a%b);
    }

}
```
