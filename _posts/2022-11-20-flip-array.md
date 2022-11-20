---
layout: post
title: Flip Array
date: 2022-11-20 15:32 +0530
author: "Gaurav Kumar"
tags: "java interviewbit dynamic_programming important"
categories: "dynamic_programming"
---

## Problem Description

Given an array of  positive elements, you have to flip the sign of some of its elements such that the resultant sum of the elements of array should be minimum non-negative(as close to zero as possible). Return the minimum no. of elements whose sign needs to be flipped such that the resultant sum is minimum non-negative.  

> Constraints:  
> 1 <= n <= 100  
> Sum of all the elements will not exceed 10,000.

[interviewbit](https://www.interviewbit.com/problems/flip-array/)

### Solution

We basically need a subnet which has the sum of all elements = half of total sum in the given list. This problem can be reduced to a Knapsack problem where we have to fill a Knapsack of capacity (S/2) as fully as possible and using the minimum no. of elements.

There can be multiple such subsets, so we need to choose the one with minimum size. So, in our solution, we will need to track two things in our dp matrix: sum of chosen elements in the subset and size of the subset.

```java
public class Solution {

    //The sum of two subsets can be same. Choose the one with lesser size
    public Pair choose(Pair x, Pair y){
        if(x.sum > y.sum){
            return x; 
        }
        else if(y.sum > x.sum){
            return y;
        }else{
            if(x.size <= y.size){
                return x;
            }else{
                return y;
            }
        }
    }

    public int solve(final int[] A) {

        int n = A.length;

        int totalSum = 0;
        for(int i=0; i<n; i++) totalSum += A[i];

        int k = totalSum/2; //target

        //since we need to track the sum and subset-size
        Pair[][] dp = new Pair[n+1][k+1]; 
        for(int i=0; i<dp.length; i++){
            Arrays.fill(dp[i], new Pair(0, 0));
        }

        for(int i=1; i<=n; i++){
            for(int j=0; j<=k; j++){

                //leave
                Pair x = dp[i-1][j];

                //pick
                if(j>=A[i-1]){
                    
                    //Add current element to the sum for dp[i-1][j-A[i-1]]
                    //Add 1 to the size to the size for dp[i-1][j-A[i-1]]
                    Pair y = new Pair( dp[i-1][j-A[i-1]].sum + A[i-1] , dp[i-1][j-A[i-1]].size + 1);

                    x = choose(x, y);

                }

                dp[i][j] = x;

            }
        }

        return dp[n][k].size;

    }
}

class Pair{

    int sum;
    int size;

    Pair(int sum, int size){
        this.sum = sum;
        this.size = size;
    }
    
}
```
