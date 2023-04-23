---
layout: post
title: Length of longest consecutive ones
date: 2023-04-23 17:34 +0530
tags: "java arrays"
categories: "arrays"
---

### PROBLEM DESCRIPTION

Given a binary string A. It is allowed to do at most one swap between any 0 and 1. Find and return the length of the longest consecutive 1’s that can be achieved.

### SOLUTION

```java
public class Solution {
    
    public int solve(String s) {

        int n = s.length();

        //convert string to integer array
        int[] A = new int[n];
        for(int i=0; i<n; i++){
            A[i] = s.charAt(i)=='0'?0:1;
        }

        //number of consecutive 1s on the left of the element
        int[] left = new int[n];

        //number of consecutive 1s on the right of the element
        int[] right = new int[n];

        //there is no element on the left of first
        left[0] = 0;

        //there is no element on the right of last 
        right[n-1] = 0;

        //calculate number of consecutive 1s for the other elements on the left of it
        for(int i=1; i<n; i++){

            //if the previous element was 1, add 1
            if(A[i-1] == 1){
                left[i] = left[i-1] + 1;
            //else reset to 0
            }else{
                left[i] = 0;
            }
        }

        //calculate number of consecutive 1s for the other elements on the right of it
        for(int i=n-2; i>=0; i--){

            //if the previous element was 1, add 1
            if(A[i+1] == 1){
                right[i] = right[i+1] + 1;
            //else reset to 0
            }else{
                right[i] = 0;
            }
        }

        //total number of 1s present in the given array
        int countOfOne = 0;
        for(int i=0; i<n; i++) if(A[i] == 1) countOfOne++;

        //if the number of 1s is equal to the array size, we can return N
        if(countOfOne == n) return n; //edge case

        //init: min possible answer is 0 if there are no 1s
        int maxLength = 0;

        //for each element, check what is the max length with consecutive 1s possible and keep updating maxLength
        for(int i=0; i<n; i++){

            //if the current element is 0, check if it can be replaced with 1
            if(A[i] == 0){
                
                //number of 1s on its left
                int l = left[i];
                //number of 1s on its right
                int r = right[i];

                //we can replace it with 1 only if there are any extra 1s remaining
                //to find that, add the number of 1s on left and right and check if the sum if less than the total number of 1s in the array
                //if it's lesser, there must be an extra 1 somewhere which can be swapped
                boolean anyOneRemaining = (countOfOne-(l+r))>0;

                //if there is an extra 1 which can be swapped, the total length possible would be: L+R+1
                if(anyOneRemaining){
                    maxLength = Math.max(maxLength, l+r+1);
                //if extra 1 is not present, then the best possible option is to take the left most or right most 1 and swap it with 0
                //in that case, the length will be l+r
                //Example: 1110111 -> here we can take the left most or right most 1 and swap with 0
                }else{
                    maxLength = Math.max(maxLength, l+r);
                }

            }
        }

        return maxLength;


    }

}
```
