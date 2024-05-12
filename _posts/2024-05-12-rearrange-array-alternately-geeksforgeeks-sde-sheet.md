---
layout: post
title: Rearrange Array Alternately (geeksforgeeks - SDE Sheet)
date: 2024-05-12 23:16 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks arrays sde-sheet important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a sorted array of positive integers. Your task is to rearrange the array elements alternatively i.e first element should be max value, second should be min value, third should be second max, fourth should be second min and so on.
Note: Modify the original array itself. Do it without using any extra space. You do not have to return anything.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/-rearrange-array-alternately-1587115620/1?page=2)

## SOLUTION

[Good Explanation](https://www.youtube.com/watch?v=kQrezgskpho)

![snapshot]({{ site.baseurl }}/assets/img/rearrange_array_alternately.png)

```java
class Solution{

    // temp: input array
    // n: size of array
    //Function to rearrange  the array elements alternately.
    public static void rearrange(long arr[], int n){

        int minIndex = 0;
        int maxIndex = n-1;

        long M = arr[n-1] + 1;

        for(int i=0; i<n; i++){

            if(i%2 == 0){

                arr[i] = (arr[maxIndex]%M)*M + arr[i];
                maxIndex--;

            }else{

                arr[i] = (arr[minIndex]%M)*M + arr[i];
                minIndex++;

            }

        }

        for(int i=0; i<n; i++)
            arr[i] = arr[i]/M;

    }

}
```
