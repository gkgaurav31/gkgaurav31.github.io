---
layout: post
title: Largest number possible (geeksforgeeks - SDE Sheet)
date: 2024-09-21 12:40 +0530
author: "Gaurav Kumar"
tags: "java strings geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given two numbers 'N' and 'S' , find the largest number that can be formed with 'N' digits and whose sum of digits should be equals to 'S'. Return -1 if it is not possible.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/largest-number-possible5028/1?page=8)

## SOLUTION

Edge Cases:

- If the sum is 0 and N > 1, it is impossible to form such a number, as the only valid number would be 0, which can only be represented as a single digit.
- If the sum S exceeds the maximum possible sum with N digits, it is not possible to form such a number.

Main Steps:

- Start with an empty result and iterate N times to construct the number.
- For each digit:
  - If the remaining sum is greater than or equal to 9, append 9 to the result. Reduce the target sum by 9.
  - If the remaining sum is less than 9, append the remaining sum itself and set the sum to 0, ensuring that subsequent digits are zeros, as no further changes to the sum are needed.

```java
class Solution{

    static String findLargest(int N, int S){

        StringBuffer sb = new StringBuffer();

        // edge case:
        // if the total sum needed is 0, and we need to use more than 1 digit
        // return -1;
        if(S == 0 && N > 1)
            return "-1";

        // edge case:
        // if the maximum sum possible using N digits (by using all 9s) is still less than target sum
        // return -1;
        if(N * 9 < S)
            return "-1";

        // loop N times to form the N digit number
        for(int i=0; i<N; i++){

            // if the target value is more than 9, append 9 to the current number
            if(S >= 9){
                sb.append("9");
                // reduce target sum by 9
                S -= 9;

            // otherwise, append the remaining value in S which will be lower than 9
            // if there are more digits left, they need to be appended as 0, so we set the S value to 0
            }else{

                sb.append(String.valueOf(S));
                S = 0;

            }

        }

        return sb.toString();

    }

}
```
