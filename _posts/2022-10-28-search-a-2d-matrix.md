---
layout: post
title: Search a 2D Matrix
date: 2022-10-28 23:18 +0530
author: "Gaurav Kumar"
tags: "java  binary_search search"
categories: "search"
---

## Problem Description

Write an efficient algorithm that searches for a value target in an m x n integer matrix matrix. This matrix has the following properties:

- Integers in each row are sorted from left to right.
- The first integer of each row is greater than the last integer of the previous row.

[leetcode](https://leetcode.com/problems/search-a-2d-matrix/)

### Solution

![snapshot]({{ site.baseurl }}/assets/img/matrix_binary_search_1.png)

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        
        int m=matrix.length;
        int n=matrix[0].length;
        
        int l=0;
        int r=m*n-1;
        
        while(l<=r){
            
            int mid = (l+r)/2;
            
            int row = mid/n; // 5/4 and 5%4
            int column = mid%n;
            
            int x = matrix[row][column];
            
            if(x == target) return true;
            
            if(x < target){
                l=mid+1;
            }else{
                r=mid-1;
            }
            
        }
        
        return false;
    }
}
```
