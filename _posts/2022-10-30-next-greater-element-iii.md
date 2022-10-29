---
layout: post
title: Next Greater Element III
date: 2022-10-30 00:33 +0530
author: "Gaurav Kumar"
tags: "java permutation"
categories: "permutation"
---

## Problem Description

Given a positive integer n, find the smallest integer which has exactly the same digits existing in the integer n and is greater in value than n. If no such positive integer exists, return -1.  

Note that the returned integer should fit in 32-bit integer, if there is a valid answer but it does not fit in 32-bit integer, return -1.  

[leetcode](https://leetcode.com/problems/next-greater-element-iii/)

### Solution

![snapshot]({{ site.baseurl }}/assets/img/next_greater_permutation.png)

```java
class Solution {
    
    public int nextGreaterElement(int n) {
        
        int[] num = new int[String.valueOf(n).length()];
        
        int temp = n;
        
        for(int i=num.length-1; i>=0; i--){
            int digit = temp%10;
            temp = temp/10;
            num[i] = digit;
        }
        
        System.out.println(Arrays.toString(num));
        
        //Decreasing element
        int a = -1;
        for(int i=num.length-2; i>=0; i--){
            if(num[i] < num[i+1]){
                a = i;
                break;
            }
        }
        
        System.out.println("Decreaing index: " + a);
        
        if(a == -1) return -1;
        
        //First element more than num[a] from right
        int b = -1;
        for(int i=num.length-1; i>a; i--){
            if(num[i] > num[a]){
                b = i;
                break;
            }
        }
        
        swap(a, b, num);
        
        reverseFrom(a+1, num);
        
        return convert(num);
            
    }
    
    public int convert(int[] arr){
        
        StringBuilder sb = new StringBuilder();
        
        for(int i=0; i<arr.length; i++){
            sb.append(arr[i]+"");
        }
        
        try{
            return Integer.parseInt(sb.toString());
        }catch(Exception e){
            return -1;
        }
        
        
    }
    
    public void reverseFrom(int i, int[] arr){
        
        int start=i;
        int end=arr.length-1;
        
        while(start<end){
            swap(start, end, arr);
            start++;
            end--;
        }
        
    }
    
    public void swap(int i, int j, int[] arr){
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
    
    
}
```
