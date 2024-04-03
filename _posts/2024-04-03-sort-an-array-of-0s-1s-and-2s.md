---
layout: post
title: Sort an array of 0s, 1s and 2s
date: 2024-04-03 23:21 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks arrays important"
categories: "arrays"
---

## PROBLEM DESCRIPTION

Given an array of size N containing only 0s, 1s, and 2s; sort the array in ascending order.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/sort-an-array-of-0s-1s-and-2s4231/1?itm_source=geeksforgeeks&itm_medium=article&itm_campaign=bottom_sticky_on_article)

## SOLUTION

This is a very interesting question and the optimal solution is not easy to come up with. We are supposed to solve this problem in linear time. So we cannot use any of the sorting algorithms like Merge Sort etc.

Let us first look into the simpler solution which uses two iterations.

### APPROACH 1

```java
class Solution
{
    public static void sort012(int a[], int n)
    {

        // Create an array to store the frequency of 0s, 1s, and 2s
        int[] freq = new int[3];

        // Store frequency of 0, 1, and 2 in their corresponding index in the frequency array
        for(int i=0; i<n; i++)
            freq[a[i]]++;

        // Update the original array with the sorted order based on the frequency of 0s, 1s, and 2s

        // Update 0s based on their frequency
        for(int i=0; i<freq[0]; i++)
            a[i] = 0;

        // Update 1s based on their frequency
        for(int i=freq[0]; i<freq[0]+freq[1]; i++)
            a[i] = 1;

        // Update 2s based on their frequency
        for(int i=freq[0]+freq[1]; i<freq[0]+freq[1]+freq[2]; i++)
            a[i] = 2;

    }
}

```

### APPROACH 2 - OPTIMAL

[takeUForward - Good Explanation](https://www.youtube.com/watch?v=tp8JIuCXBaU)

The idea is to do the following:

- We need to use a `low` pointer before which all the numbers should be 0.
- We need to use a `high` pointer after which all the numbers should be 2.
- We need to use a `mid` pointer which will help in checking the elements one by one and putting them in the right position

If we can make sure that all the numbers before low are 0, and all numbers after high are 2, then we have already sorted these two parts. So, we just need to sort the remaining unsorted path from [mid, high-1]

```java
class Solution
{
    public static void sort012(int a[], int n)
    {

        int low = 0; // all numbers before low will be 0
        int mid = 0; // help with iteration
        int high = n-1; // all numbers after high will be 2

        // high will continue to decrease whenever we get a 2
        // mid will continue to increase during iterations
        // we will continue doing so until mid<=high
        while(mid <= high){

            // if element at mid is 0
            // we need to bring it to the front, so swap elements at low and mid
            // now, low can move one step forward
            // to check the next element, we move mid as well
            if(a[mid] == 0){
                swap(a, low, mid);
                mid++;
                low++;

            // this one is important
            // if the current element at mid is 2
            // we need to move it to the right side
            // we can swap the elements at mid and high
            // then, we can definitely reduce the value of high (since we want all numbers on right of high to be 2)
            // but we might have brought in a different number which can be 0/1/2 from index high
            // so we should not really increase the mid pointer this time
            // it will get handled in the next iteration
            }else if(a[mid] == 2){
                swap(a, high, mid);
                high--;

            // if the element at mid is 1, nothing to do actually since all the 1s will eventually get sorted
            }else{
                mid++;
            }

        }
    }

    public static void swap(int[] a, int i, int j){
        int t = a[i];
        a[i] = a[j];
        a[j] = t;
    }

}
```
