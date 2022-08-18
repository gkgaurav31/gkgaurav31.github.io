---
layout: post
title: Kth Element (Without Sorting)
date: 2022-08-19 00:05 +0530
author: "Gaurav Kumar"
tags: "java search important"
categories: "search"
---

## Problem Description

Given unsorted array of N distinct elements, find kth index position of its sorted form. (k will start from 0)
Note: We cannot modify the array and we cannot use extra space.  

> Example:  
> arr[5] = {2 8 3 11 14}  
> k=2  
> Answer: 8  

### Solution

```java
package com.gauk;

public class KthElement {

    public static void main(String[] args) {
        System.out.println(new KthElement().findKthElement(new int[]{11, 24, 20, 3, 5, 27, 34, 9, 40}, 4));
    }

    public int findKthElement(int[] arr, int k){

        //The search space can be between the min and max of the array
        int l=getMin(arr);
        int h=getMax(arr);

        //initialize the answer with any value
        int ans = l;

        while(l<=h){

            int m = (l+h)/2;

            //how many elements less than M are present in the array?
            int c = elementsLessThan(arr, m);

            //we are looking for kth element. there must be exactly k elements lesser than it in sorted form [not k-1 because we are starting from 0]
            if(c < k){

                //the number of elements less than m in the array is less than k. So, for any number lesser than m, the value will be equal to or even lower.
                //so we can discard left side
                l=m+1;

            }else if(c > k){

                //the number of elements more than m in the array is more than k. This will be true for any number more than m. So, we can discard right side.
                h=m-1;

            }else{

                //This part is important. It may seem that we can simply return the answer here. However, it is possible that the number is not even present in the array.
                //So, we need to save the answer, and look for another number.
                //The important thing to realize here is that we would need to go to the right to look for the correct answer.
                //This is because, the  right most number which has k elements smaller than itself is the one present in the answer. The next number must be having > k elemeents smaller than itself.
                ans = m;
                l=m+1;

            }

        }

        return ans;

    }

    public int elementsLessThan(int[] arr, int m){
        int c = 0;
        for(int i=0; i<arr.length; i++){
            if(arr[i] < m) c++;
        }
        return c;
    }

    public int getMin(int[] arr){
        int min = arr[0];
        for(int i=0; i<arr.length; i++){
            if(arr[i] < min) min = arr[i];
        }
        return min;
    }

    public int getMax(int[] arr){
        int max = arr[0];
        for(int i=0; i<arr.length; i++){
            if(arr[i] > max) max = arr[i];
        }
        return max;
    }

}
```
