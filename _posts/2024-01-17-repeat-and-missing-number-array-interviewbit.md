---
layout: post
title: Repeat and Missing Number Array (InterviewBit)
date: 2024-01-17 21:10 +0530
author: "Gaurav Kumar"
tags: "java interviewbit arrays"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

You are given a read only array of n integers from 1 to n.

Each integer appears exactly once except A which appears twice and B which is missing.

Return A and B.

Note: Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

Note that in your output A should precede B.

[InterviewBit](https://www.interviewbit.com/problems/repeat-and-missing-number-array/)

## SOLUTION

### APPROACH 1 - USING HASHMAP

```java
public class Solution {

    public int[] repeatedNumber(final int[] A) {

        int n = A.length;

        // Create a HashMap to store the frequency of each integer in the array
        Map<Integer, Integer> map = new HashMap<>();
        for(int i=0; i<n; i++){
            map.put(A[i], map.getOrDefault(A[i], 0) + 1);
        }

        // Initialize an array to store the two numbers A and B
        int[] ans = new int[]{-1, -1};

        // Iterate through the numbers from 1 to n
        for(int i=1; i<=n; i++){

            // Initialize a variable to store the frequency of the current number
            int freq = 0;

            // Check if the current number is present in the HashMap and get its frequency
            if(map.containsKey(i))
                freq = map.get(i);

            // If the frequency is 0, it means the missing number is found
            if(freq == 0){
                ans[1] = i; // Set the missing number (B)
            }

            // If the frequency is 2, it means the repeated number is found
            if(freq == 2){
                ans[0] = i; // Set the repeated number (A)
            }

            // If both A and B are found, exit the loop
            if(ans[0] != -1 && ans[1] != -1) break;

        }

        return ans;

    }
}
```

### APPROACH 2 - MARKING PRESENT VALUES IN THE ARRAY

```java
public class Solution {

    public int[] repeatedNumber(final int[] A) {

        // Get the length of the array
        int n = A.length;

        // Variable to store the repeated number
        int repeatedNumber = -1;

        // Iterate through the elements of the array
        for(int i=0; i<n; i++){

            // Take the absolute value of the current element
            int x = Math.abs(A[i]);

            // If the element at position (x-1) is already negative, it means x is the repeated number
            if(A[x-1] < 0){
                repeatedNumber = x;
            }else{
                // Mark the element at position (x-1) as negative to indicate its presence
                A[x-1] = -A[x-1];
            }
        }

        // Variable to store the missing number
        int missingNumber = -1;

        // Iterate through the modified array to find the positive element, indicating the missing number
        // if the numbeer at index i is positive (not marked), that means the number (i+1) was not found in the array
        for(int i=0; i<n; i++){
            if(A[i] > 0){
                missingNumber = i+1; // Set the missing number
                break;
            }
        }

        return new int[]{repeatedNumber, missingNumber};

    }
}
```

### APPROACH 3 - USING SOME MATHS

Suppose A and B are the numbers which we need to find. One of them is repeated and another is missing.

The main observations are:

- sum of numbers from [1, N] - sum of numbers in the array = Math.abs(A - B) = X
- (sum of squares of numbers from [1,N] - sum of squares of numbers in the arrays)= A^2 - B^2  
   = (A+B)*(A-B)  
   = (A+B)*X  
  => (sum of squares of numbers from [1,N] - sum of squares of numbers in the arrays)/X = (A+B) = Y

So, we have two equations ->

> A - B = Math.abs(sumOfN - sumOfArray)
> A + B = Math.abs(sumOfSquareN - sumOfSquareArray)/(A-B)

If we add both these equations:

> 2A = X + Y  
> => A = (X+Y)/2;

Substitue value of A in A - B = Math.abs(sumOfN - sumOfArray):

> A - B = Math.abs(sumOfN - sumOfArray)
> B = A - Math.abs(sumOfN - sumOfArray)

But which one is repeated and which one is missing?

Well, if there was a higher number which was repeated, the sum of numbers in the array will exceed sum of numbers from [1,N]. We can use this observation to create our answer array.

Example:

```text
arr = [1 2 3 5 5]

sum of N = 5*6/2 = 15
sum of numbers in array = 16 > sum of N

so the larger number out of A, B should be the repeated one.
```

```java
public class Solution {

    public int[] repeatedNumber(final int[] A) {

        int n = A.length;

        // Calculate the sum of first n natural numbers
        long sumOfN = sumOfN(n);

        // Calculate the sum of squares of first n natural numbers
        long sumOfSquareN = sumOfSquareN(n);

        // Calculate the sum of the given array
        long sumOfArray = sumOfArray(A);

        // Calculate the sum of squares of the elements in the given array
        long sumOfSquareArray = sumOfSquareArray(A);

        //X = A - B = Math.abs(sumOfN - sumOfArray)
        //Y = A + B = Math.abs(sumOfSquareN - sumOfSquareArray)/(A-B)
        //2A = X + Y => A = (X+Y)/2;
        //B = A - X;

        long x = Math.abs(sumOfN - sumOfArray);
        long y = Math.abs(sumOfSquareN - sumOfSquareArray) / x;

        long a = (x + y) / 2;
        long b = a - x;

        // if sumOfArray > sumOfN, the larger number must be the one which is repeated
        if (sumOfArray > sumOfN) {
            return new int[]{Math.max((int) a, (int) b), Math.min((int) a, (int) b)};
        } else {
            return new int[]{Math.min((int) a, (int) b), Math.max((int) a, (int) b)};
        }
    }

    // Helper method to calculate the sum of first n natural numbers
    public static long sumOfN(int n) {
        long sum = 0;
        for (long i = 1; i <= n; i++) {
            sum += i;
        }
        return sum;
    }

    // Helper method to calculate the sum of squares of first n natural numbers
    public static long sumOfSquareN(int n) {
        long sum = 0;
        for (long i = 1; i <= n; i++) {
            sum += i * i;
        }
        return sum;
    }

    // Helper method to calculate the sum of elements in an array
    public static long sumOfArray(int[] array) {
        long sum = 0;
        for (int num : array) {
            sum += num;
        }
        return sum;
    }

    // Helper method to calculate the sum of squares of elements in an array
    public static long sumOfSquareArray(int[] array) {
        long sum = 0;
        for (int num : array) {
            sum += (long) num * num;
        }
        return sum;
    }
}
```
