---
layout: post
title: Largest Number (InterviewBit)
date: 2024-01-21 21:24 +0530
author: "Gaurav Kumar"
tags: "java interviewbit arrays"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given a list of non-negative integers, arrange them such that they form the largest number.
Note: The result may be very large, so you need to return a string instead of an integer.

[InterviewBit](https://www.interviewbit.com/problems/largest-number/)

## SOLUTION

We can use a custom `Comparator` which will help in sorting the values in the array such that we get the largest value. It will take any two values from the given array, say `A` and `B`, and check which one forms a larger value: `A -> B` or `B -> A`. Both `A` and `B` can also be same, in which the order will not matter.

For example:

`A` -> 89  
`B` -> 55

`AB` -> 8955 (after concatenation)  
`BA` -> 5589 (after concatenation)

Now, we compare these two strings `AB` and `BA` one digit at a time and pick up the largest one.

```java
import java.util.Arrays;
import java.util.Comparator;

public class Solution {

    public String largestNumber(final int[] A) {

        int n = A.length;

        // Convert the array of ints to an array of Strings for sorting based on custom comparator
        String[] arr = new String[n];
        for(int i = 0; i < n; i++)
            arr[i] = String.valueOf(A[i]);

        // Sort the array using the custom comparator
        Arrays.sort(arr, new MyComparator());

        // StringBuffer to build the result string
        StringBuffer sb = new StringBuffer();

        // To handle the edge case where all elements are 0 (e.g., [0, 0, 0, 0, 0])
        boolean allZeroes = true;

        // Build the result string and check if all elements are 0
        for(int i = 0; i < n; i++){
            sb.append(arr[i] + "");

            if(!arr[i].equals("0"))
                allZeroes = false;
        }

        // If all elements are 0, return "0" as the result
        if(allZeroes)
            return "0";

        // Return the final result string
        return sb.toString();
    }
}

// Custom comparator class for sorting Strings based on forming the largest number
class MyComparator implements Comparator<String> {

    @Override
    public int compare(String o1, String o2) {

        // Concatenate strings for comparison
        String s1 = o1 + o2;
        String s2 = o2 + o1;

        int n = s1.length();
        int m = s2.length();

        // Compare the concatenated strings
        // If length of s1 is larger, return -1; if length of s2 is larger, return 1; if they are equal, continue checking individual digits
        if(n > m) {
            return -1;
        } else if(n < m) {
            return 1;
        } else {

            // Check individual digits
            for(int i = 0; i < n; i++) {

                int a = Integer.parseInt(s1.charAt(i) + "");
                int b = Integer.parseInt(s2.charAt(i) + "");

                if(a > b) {
                    return -1;
                } else if(b > a) {
                    return 1;
                }
            }

            // If all digits are equal, return 0
            return 0;
        }
    }
}
```

### ANOTHER WAY TO CODE

We can just compare using `(s2+s1).compareTo(s1+s2)`.

```java
import java.util.Arrays;
import java.util.Comparator;

public class Solution {

    public String largestNumber(final int[] A) {

        int n = A.length;

        String[] arr = new String[n];
        for(int i=0; i<n; i++) arr[i] = String.valueOf(A[i]);

        Arrays.sort(arr, new Comparator<String>() {
            @Override
            public int compare(String o1, String o2) {
                return (o2+o1).compareTo(o1+o2);
            }
        });

        StringBuffer sb = new StringBuffer();

        for(int i=0; i<n; i++){
            sb.append(arr[i] + "");
        }

        // remove trailing zeroes, unless the result is 0
        while(sb.charAt(0) == '0' && sb.length() > 1)
            sb.deleteCharAt(0);

        return sb.toString();

    }


}
```
