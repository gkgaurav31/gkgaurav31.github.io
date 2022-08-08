---
layout: post
title: Radix Sort
date: 2022-08-09 00:55 +0530
author: "Gaurav Kumar"
tags: "java sorting"
categories: "sorting"
---

## Problem Description

Implement Radix Sort.

### Solution

- Find the number of digits of the max element in the given array
- Initialize an Array "bucket" of ArrayList<Integer> with size 10. This will be used for storing the numbers after comparing them using value at any index.
- If the max element has 5 digits, for example, we loop 5 times from 0th index to 4th index (from right to left). Take each number in the array. Get the digit at ith index. If the value is 0, put it in the 0th index of "bucket". If it's 1, put it in the 1st index of the bucket and so on. There can be multiple elements, which go to the same bucket. 
- Once we have all the numbers in the bucket, loop through the bucket and add them back to the given array. We will get sorted array with index 0. Do the same thing for index 1, then index 2, and so on. 
- We will finally get a sorted array.

```java
import java.util.*;

class Program {

  public ArrayList<Integer> radixSort(ArrayList<Integer> array) {

    if(array.size() <= 1) return array;

    //Array of ArrayList of size 10 -> [0 to 9]
    @SuppressWarnings("unchecked")
    ArrayList<Integer>[] bucket = (ArrayList<Integer>[]) new ArrayList[10];
    for(int i=0; i<bucket.length; i++){
      bucket[i] = new ArrayList<Integer>();
    }
    
    int maxElement = getMaxElement(array);
    int digits = getNumberOfDigitsOfMaxElement(maxElement);

    //Keep sorting based on value at 0th index, 1st index, 2nd index and so on...
    for(int i=0; i<digits; i++){

      for(int s=0; s<bucket.length; s++){
        bucket[s] = new ArrayList<Integer>();
      }

      for(int j=0; j<array.size(); j++){
        int num = array.get(j);
        int digit = (num/(int)Math.pow(10,i))%10;
        bucket[digit].add(num);
      }

      int k=0;
      for(int j=0; j<bucket.length; j++){
        for(int w=0; w<bucket[j].size(); w++){
          array.set(k, bucket[j].get(w));
          k++;
        }
      }
      
    }

    return array;
    
  }

  public int getMaxElement(ArrayList<Integer> array){
    int max = array.get(0);
    for(int i=1; i<array.size(); i++){
      if(array.get(i)>max) {
        max = array.get(i);
      }
      
    }
    return max;
  }

  public int getNumberOfDigitsOfMaxElement(int n){

    int c = 0;
    while(n!=0){
      n = n/10;
      c++;
    }

    return c;
    
  }
  
}
```
