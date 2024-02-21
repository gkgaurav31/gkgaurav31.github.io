---
layout: post
title: Different Bits Sum Pairwise (InterviewBit)
date: 2024-02-21 21:24 +0530
tags: "java bit_manipulation important"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

We define `f(X, Y)` as the number of different corresponding bits in the binary representation of `X` and `Y`.

For example, `f(2, 7) = 2`, since the binary representation of `2` and `7` are `010` and `111`, respectively. The first and the third bit differ, so `f(2, 7) = 2`.

You are given an array of N positive integers, `A1, A2,..., AN`. Find sum of `f(Ai, Aj)` for all pairs `(i, j)` such that `1 ≤ i, j ≤ N`. Return the answer modulo `10^9+7`.

## SOLUTION

The main idea -

For position `i`, let us say there are x elements which has `0` (unset bit).
For position `i`, let us say there are y elements which has `1` (set bit).

Then, number of pairs which will contribute to the answer will be:
`2 * (x*y)`. We multiple with `2` since for positions `i, j`, we need to consider both `(A[i], A[j])` and `(A[j], A[i])`.

```java
public class Solution {

    int M = (int) Math.pow(10,9) + 7;

    public int cntBits(int[] A) {

        int n = A.length;
        long ans = 0;

        // Iterate through each bit position (from 0 to 31)
        for(int i=0; i<32; i++){

            // Counter for the number of zeros at bit position i
            long countZero = 0;
            // Counter for the number of ones at bit position i
            long countOne = 0;

            // Iterate through each integer in the array
            for(int j=0; j<n; j++){

                // Check if the bit at position i in the binary representation of A[j] is set
                if( (A[j]&(1<<i)) !=0 ){
                    // Increment countOne if the bit is set
                    countOne++;
                }else{
                    // Increment countZero if the bit is not set
                    countZero++;
                }

            }

            // If there are x number of elements with unset bit
            // And there are y number of elements with set bit
            // There will be total 2 * (x * y) number of pairs that can be created
            // We multiply by 2 since for positions i,j we need to consider both (i,j) and (j,i)
            long pairContribution = (countZero%M * countOne%M)%M;
            long totalContribution = (2%M * pairContribution%M)%M;

            // Add the total contribution at bit position i
            ans = (ans%M + totalContribution%M)%M;

        }

        return (int) ans;

    }

}
```
