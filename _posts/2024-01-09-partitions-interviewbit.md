---
layout: post
title: Partitions (InterviewBit)
date: 2024-01-09 20:57 +0530
Author: "Gaurav Kumar"
tags: "java arrays interviewbit"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

You are given a 1D integer array B containing A integers.

Count the number of ways to split all the elements of the array into **3 contiguous parts** so that the sum of elements in each part is the same.

Such that : `sum(B[1],..B[i]) = sum(B[i+1],...B[j]) = sum(B[j+1],...B[n])`

[interviewbit](https://www.interviewbit.com/problems/partitions/)

## SOLUTION

```java
public class Solution {

    public int solve(int A, int[] B) {

        int n = B.length;

        int totalSum = getTotalSum(B);

        //the total sum should be divisible by 3 so that 3 partitions with equal sum can be made
        if(totalSum%3 != 0) return 0;

        //ans: total num of ways to create three partitions with equal sum
        int count = 0;

        //each partition must have sum as totalSum/3
        int neededPartitionSum = totalSum/3;

        //sum of first partition
        int firstPartitionSum = 0;

        for(int i=0; i<n; i++){

            firstPartitionSum += B[i];

            //if sum equals totalSum/3, first partition found
            if(firstPartitionSum == neededPartitionSum){

                //sum of second partition
                int secondPartitionSum = 0;

                //start from index after the end of first partition
                for(int j=i+1; j<n; j++){

                    secondPartitionSum += B[j];

                    //second partition found
                    if(secondPartitionSum == neededPartitionSum){

                        //the sum of third partition will be
                        //totalSum - sum of first partition - sum of third partition
                        //= 3x - x - x = x
                        //edge case: first partition and second partition use all the elements in the array
                        //so, the third partition cannot be created

                        //if there are more elements using which 3rd partition can be made, increase the count
                        if(j < n-1) count++;


                    }

                }


            }

        }

        return count;


    }

    public int getTotalSum(int[] A){
        int s = 0;
        for(int i=0; i<A.length; i++) s += A[i];

        return s;
    }

}
```
