---
layout: post
title: Balance Array (Special Index)
date: 2023-04-21 21:03 +0530
tags: "java arrays important interviewbit"
categories: "arrays"
---

### PROBLEM DESCRIPTION

Given an integer array A of size N. You need to count the number of special elements in the given array.
A element is special if removal of that element make the array balanced.
Array will be balanced if sum of even index element equal to sum of odd index element.

[interviewbit](https://www.interviewbit.com/problems/balance-array/)

### SOLUTION

The most important observation needed to solve this problem is to realize is:
If the ith index element gets deleted, all the elements on its right will move left by one positon => This means, all even index will become odd and vice versa.

We use this idea to get the sum of even position elements and odd position elements.  

Let's say, element at ith index is removed. Then:  

```Sum of elements in even position = (Sum of elements in EVEN index from 0 to i-1) + (Sum of elements in ODD index from i+1 to N-1)```

Similarly,

```Sum of elements in odd position = (Sum of elements in ODD index from 0 to i-1) + (Sum of elements in EVEN index from i+1 to N-1)```

To optimize the calculatation of sum of elements in a given range, we will use prefix array.

```java
public class Solution {
    
    public int solve(int[] A) {
        
        int n = A.length;
        
        //prefix sum for even and odd position elements
        
        //Example: [1, 5, 3, 6, 8, 2]
        //pEven: [1, 1, 4, 4, 12, 12]
        //pOdd:[0, 5, 5, 11, 11, 13]

        int pEven[] = new int[n];
        int pOdd[] = new int[n];
        
        //init (0 is even)
        pEven[0] = A[0];
        pOdd[0] = 0;
        
        //create the pEven prefix array
        for(int i=1; i<n; i++){
            if(i%2 == 0)
                pEven[i] = pEven[i-1] + A[i];
            else
                pEven[i] = pEven[i-1];
        }
        
        //create the pOdd prefix array
        for(int i=1; i<n; i++){
            if(i%2 == 1)
                pOdd[i] = pOdd[i-1] + A[i];
            else
                pOdd[i] = pOdd[i-1];
        }
        
        //init: count of special index
        int count = 0;
        
        //Check the even and odd sum if ith index is removed
        //Increase the count if they are same for any index
        for(int i=0; i<n; i++){
            
            //init
            int evenSum = 0;
            int oddSum = 0;
            
            //for first index, there is nothing towards the left
            if(i == 0){
                evenSum = rangeSum(A, i+1, n-1, pOdd);
                oddSum = rangeSum(A, i+1, n-1, pEven);
            }else{

                //even sum: sum of elements at even positions on the left and odd positions on the right
                evenSum = rangeSum(A, 0, i-1, pEven) + rangeSum(A, i+1, n-1, pOdd);

                //odd sum: sum of elements at odd positions on the left and even positions on the right
                oddSum = rangeSum(A, 0, i-1, pOdd) + rangeSum(A, i+1, n-1, pEven);  
            }
            
            //increase count if they are equal
            if(evenSum == oddSum) count++;
            
        }
        
        return count;
        
    }
    
    //given a prefix array, return the sum of elements from index L to index R
    public int rangeSum(int[] A, int L, int R, int[] pf){
        
        if(L == 0)
            return pf[R];
        else
            return pf[R] - pf[L-1];
        
    }
    
}
```
