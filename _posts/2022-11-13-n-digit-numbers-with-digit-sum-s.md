---
layout: post
title: N digit numbers with digit sum S
date: 2022-11-13 19:19 +0530
author: "Gaurav Kumar"
tags: "java interviewbit dynamic_programming"
categories: "dynamic_programming"
---

## Problem Description

Find out the number of N digit numbers, whose digits on being added equals to a given number S. Note that a valid number starts from digits 1-9 except the number 0 itself. i.e. leading zeroes are not allowed.  

Since the answer can be large, output answer modulo 1000000007  

```plaintext
N = 2, S = 4
Valid numbers are {22, 31, 13, 40}
Hence output 4.
```

[interviewbit](https://www.interviewbit.com/problems/n-digit-numbers-with-digit-sum-s-/)  

### Solution

```java
public class Solution {

    int M = 1000000007;

    public int solve(int A, int B) {
        
        //dp[i][j] will mean, possible numbers with i digits and j sum
        int[][] dp = new int[A+1][B+1];

        //initialize with -1
        for(int i=0; i<dp.length; i++){
            Arrays.fill(dp[i], -1);
        }

        //initialize answer 0

        int ans = 0;

        //The first digit cannot be 0 so we are looping from 1 to 9 here
        //Then we are calling our helper function to find the count of numbers with digits (A-1) and sum (B-i)
        for(int i=1; i<=9; i++){
            ans = (ans%M + calcHelper(A-1, B-i, dp)%M)%M;
        }

        return ans;
        
    }
    
    //return count of numbers with N digits which sum up to "sum"
    public int calcHelper(int n, int sum, int[][] dp){
        
        //If sum becomes less than 0, return 0
        if(sum < 0) return 0;

        //If number of remaining digits is 0, check if remaining sum is 0
        if(n == 0){

            //if sum is 0, return 1
            if(sum == 0){
                return 1;
            }

            //else return 0
            else{
                return 0;
            }
        }

        //dp[n][sum] has not been calculated
        if(dp[n][sum] == -1){
            
            //initialize count to 0
            int count=0;

            //Loop from 0 to 9 this time because we have already handled the first digit in the main method
            //The other digits can be from [0-9]
            for(int i=0; i<=9; i++){

                //For each digits, call helper function recursively with one digit less, and sum less by i which is the current digit being chosen
                count = (count%M + calcHelper(n-1, sum-i, dp)%M)%M;
                
            }

            dp[n][sum] = count;

        }

        return dp[n][sum];

    }
    
}
```
