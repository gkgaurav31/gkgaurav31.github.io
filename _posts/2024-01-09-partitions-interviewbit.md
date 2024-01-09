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

### BETTER APPROACH

We have to form three partitions in an array:
`(partition one)(partition two)(partition three)`

We know that sum of each partition will be totalSum/3. If totalSum is not divisible by 3, simply return 0.

We can find `ALL END INDICES` of the `FIRST PARTITION` by using `totalSum/3` value, by iterating from left to right.

We can find `ALL START INDICES` of the `THIRD PARTITION` by using `totalSum/3` value, by iterating from right to left.

Now, the only thing left is to check if the `PARTITION TWO` which can be formed using any of the combination from `PARTITION ONE` end index and `PARTITION THREE` start index is valid (its sum should also be totalSum/3).

For every possible end index of partition one, say `i`, we will initialize the start of second partition which will be `i+1`.

For every possible start index of partition three, say `j`, we will initialize the end of second partition which will be `j-1`.

`(partition one)(partition two)(partition three)`
->`(0, i)(i+1, j-1)(j,n-1)`

Now the only part left is to verify if the `PARTITION TWO` from `i+1` to `j-1` has the sum as `totalSum/3`, for which we can use our `prefix sum array`. If it is, increase the count by 1.

```java
public class Solution {

    public int solve(int A, int[] B) {

        int n = B.length;

        // Calculate the total sum of the array
        int totalSum = getTotalSum(B);

        // If the total sum cannot be evenly divided into 3 parts, return 0
        if(totalSum % 3 != 0) return 0;

        // Calculate the prefix sum array of the input array B
        int[] pf = getPrefixSumArray(B);

        // Find the end indices for the first partition where the sum equals totalSum / 3
        List<Integer> findEndIndexForFirstPartition = findEndIndexForFirstPartition(B, totalSum / 3);

        // Find the start indices for the third partition where the sum equals totalSum / 3
        List<Integer> findStartIndexForThirdPartition = findStartIndexForThirdPartition(B, totalSum / 3);

        int count = 0;

        for(int i = 0; i < findEndIndexForFirstPartition.size(); i++) {

            for(int j = 0; j < findStartIndexForThirdPartition.size(); j++) {

                // There should be at least 1 element between the first and third partitions
                if(findStartIndexForThirdPartition.get(j) - findEndIndexForFirstPartition.get(i) >= 2) {

                    int startOfSecondPartition = findEndIndexForFirstPartition.get(i) + 1;
                    int endOfSecondPartition = findStartIndexForThirdPartition.get(j) - 1;

                    // Calculate the sum of the second partition using prefix sum array
                    int secondPartitionSum = rangeSum(startOfSecondPartition, endOfSecondPartition, pf);

                    // If the sum of the second partition matches the target sum, increment count
                    if(secondPartitionSum == totalSum / 3) {
                        count++;
                    }

                }
            }
        }

        return count;
    }

    // Function to calculate the total sum of an array
    public int getTotalSum(int[] A) {
        int s = 0;

        for(int i = 0; i < A.length; i++) {
            s += A[i];
        }

        return s;
    }

    // Function to find end indices for the first partition with the needed sum
    public List<Integer> findEndIndexForFirstPartition(int[] A, int neededSum) {
        int n = A.length;
        List<Integer> list = new ArrayList<>();

        int sum = 0;

        for(int i = 0; i <= n - 3; i++) {
            sum += A[i];
            if(sum == neededSum) {
                list.add(i);
            }
        }

        return list;
    }

    // Function to find start indices for the third partition with the needed sum
    public List<Integer> findStartIndexForThirdPartition(int[] A, int neededSum) {
        int n = A.length;
        List<Integer> list = new ArrayList<>();

        int sum = 0;

        for(int i = n - 1; i >= 2; i--) {
            sum += A[i];
            if(sum == neededSum) {
                list.add(i);
            }
        }

        return list;
    }

    // Function to generate the prefix sum array of an input array
    public int[] getPrefixSumArray(int[] A) {
        int n = A.length;
        int[] pf = new int[n];

        pf[0] = 0;
        for(int i = 1; i < n; i++) {
            pf[i] = pf[i - 1] + A[i];
        }

        return pf;
    }

    // Function to calculate the sum of elements within a range using the prefix sum array
    public int rangeSum(int start, int end, int[] pf) {
        if(start == 0) return pf[end];
        return pf[end] - pf[start - 1];
    }
}
```
