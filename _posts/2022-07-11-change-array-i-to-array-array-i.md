---
layout: post
title: Change Array[i] to Array[Array[i]]
date: 2022-07-11 00:26 +0530
author: "Gaurav Kumar"
tags: "java arrays modulo important"
categories: "arrays"
---

## Problem Description

Given a list of elements (an array) and a postive integer m, replace all elments such that Array[i] = Array[Array[i]]. 
All elements in the array are in the range [0, N-1].

### Solution

One of the obvious way to do this is by using extra space. We can simple create a new list and loop through the given array/list and keep updating the values needed in our answer array/list.

Let's think about how we can do it in a more optimized way without using extra space.  

> Example:  
> Array: 3 4 2 0 1  
>
> The result will be:  
> = [ A[3], A[4], A[2], A[0], A[1] ]  
> = [ 0, 1, 2, 3, 4 ]  

The challenge here is to keep a track of two things:

- The existing value in the input array
- The value at the index denoted by A[i]

Think how the following example:

- 23 hours means: 0 day, 23 hours  
- 25 hours means: 1 day, 1 hour
- 48 hours means: 2 days, 0 hour
- 50 hours means: 2 days, 2 hours
- x hours means: x/24 days, x%24 hours

Since the range of elements in the array is [0, N-1], we can somehow use this logic to save two things in our array.

To store the actual given input, let's multiple all elements by N:

```java
for(int i=0; i<n; i++){
    A.set(i, A.get(i)*n);
}
```

To get the actual value, we can just divide by N.

Now, to store the 2nd value (the value at index Array[i]), let's add Array[i] to each element. Note that we have multiplied each element by N, so we will need to retrieve that element by dividing it to get the actual value.

```java
for(int i=0; i<n; i++){
    int idx = A.get(i)/n; //get the index from which we need to get the new value
    int val = A.get(idx)/n; //get the new value from that index calculated. We divide it by N since we had multiplied by N in the previous loop
    A.set(i, val+A.get(i)); //update it by adding the retrieved value
}
```

From this new list, we can get both the actual value by dividing it by N and the value which needs to be updated to by taking mod of N.

To update the list in-place, simply add another loop:

```java
for(int i=0; i<n; i++){
    A.set(i, A.get(i)%n);
}
```

Complete Code:

```java
import java.lang.*;
import java.util.*;
import java.util.Collections;
public class Solution {
    public void arrange(ArrayList<Integer> A) {

        int n = A.size();

        for(int i=0; i<n; i++){
            A.set(i, A.get(i)*n);
        }

        for(int i=0; i<n; i++){
            int idx = A.get(i)/n;
            int val = A.get(idx)/n;
            A.set(i, val+A.get(i));
        }

        for(int i=0; i<n; i++){
            A.set(i, A.get(i)%n);
        }
        
    }
}
```
