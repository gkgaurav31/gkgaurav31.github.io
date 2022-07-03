---
layout: post
title: Largest Range in the Array
date: 2022-07-03 16:21 +0530
author: "Gaurav Kumar"
tags: "java arrays"
categories: "arrays"
---

## Problem Description

Given an array of integers, return the start and end element (not index) which represents the maximum continuous range of numbers present in the array.  
Example: 1 11 3 0 15 5 2 4 10 7 12 6  
In this, the longest range is [0,7] since all numbers between 0 to 7 are present in the give array.

### Solution

One approach to solve this is by using HashSet to reduce the lookup time for any element. We add all elements in the list to the hashset. Then we loop through the array while tacking the currentMaxCount and the corresponding startElement and EndElement. When the next element is not found in the list, we can reset the count, min and max element. Since the lookup time is constant, the overall time complexity will be O(n).

```java
import java.util.*;

class Program {
  public static int[] largestRange(int[] array) {

    HashSet<Integer> set = new HashSet<Integer>();

    int min = Integer.MAX_VALUE;
    int max = Integer.MIN_VALUE;
    
    for(int i=0;i<array.length;i++){
      set.add(array[i]);
      min = Math.min(array[i], min);
      max = Math.max(array[i], max);
    }

    int countMax = 0;
    int ansStart = -1;
    int ansEnd = -1;
    
    int currentMax = 0;
    boolean markedStart = false;
    int currentStart = array[0];
    int currentEnd = array[0];
    
    for(int i=min; i<=max; i++){

      if(set.contains(i)){

        if(markedStart){
          currentEnd = i;
        }else{
          currentStart = i;
          markedStart = true;
        }

        currentMax++;
        
      }else{

        if(currentMax > countMax){
          countMax = currentMax;
          ansStart = currentStart;
          ansEnd = currentEnd;
        }

        currentMax = 1;
        markedStart = false;
        
      }

    }

    if(currentMax > countMax){
        countMax = currentMax;
        ansStart = currentStart;
        ansEnd = currentEnd;
    }

    return new int[]{ansStart, ansEnd};

  }
  
}
```
