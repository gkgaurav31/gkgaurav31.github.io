---
layout: post
title: Equilibrium Index of an Array
date: 2022-06-14 11:44 +0530
author: "Gaurav Kumar"
tags: "java, arrays, geeksforgeeks"
categories: "arrays"
---

## Problem Description

Check if there is any index for which the sum of all elements on the left side equals sum of all elements on the right. [Equilibrium Index of an Array](https://www.geeksforgeeks.org/equilibrium-index-of-an-array/)

### Solution

We can make use of prefix array to solve this optimally. Fist, we initialize the prefix array. For every i from 0 to N-1, we get the sum on the left and sum of the right using the prefix array.  

LeftSum = prefix[i-1];  
rightSum = prefix[n-1] - prefix[i];  

:exclamation: Edge case: for i=0, handle leftSum separately.

```java
class Solution {
    String equilibrium(int arr[], int n) {
        
        int prefix[] = new int[n];
        prefix[0] = arr[0];
        
        for(int i=1; i<n; i++){
            prefix[i] = prefix[i-1] + arr[i];
        }
        
        int leftSum = 0;
        int rightSum = 0;
        String output="NO";
        
        for(int i=0; i<n; i++){
            
            if(i==0){
                leftSum = 0;
            }else{
                leftSum = prefix[i-1];
            }
            
            rightSum = prefix[n-1] - prefix[i];
            
            if(leftSum == rightSum){
                output = "YES";
                break;
            }
            
        }
        
        return output;
        
    }
}
```
