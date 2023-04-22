---
layout: post
title: Closest MinMax
date: 2023-04-22 14:19 +0530
tags: "java arrays interviewbit"
categories: "arrays"
---

### PROBLEM DESCRIPTION

Given an array A of size N. The task is to find the length of smallest subarray which contains both maximum and minimum values.

[geeksforgeeks](https://www.geeksforgeeks.org/smallest-subarray-containing-minimum-and-maximum-values/)

### SOLUTION

### APPROACH 1

First we get the min and max values. Then, iterate through the array from left to right. If we get min, look for the nearest max towards right. Similarly, if we get the max, look for nearest min towards right. Keep updating the minSize which is the answer.

```java
public class Solution {

    public int solve(int[] A) {

        int n = A.length;

        int min = Integer.MAX_VALUE;
        int max = Integer.MIN_VALUE;

        for(int i=0; i<n; i++){
            min = Math.min(min, A[i]);
            max = Math.max(max, A[i]);
        }

        if(min == max) return 1;

        int minSize = A.length;

        for(int i=0; i<n; i++){
            
            //current element is the min
            //look for nearest max toweards right
            if(A[i] == min){

                for(int j=i+1; j<n; j++){
                    if(A[j] == max){
                        minSize = Math.min(minSize, j-i+1);
                        break;
                    }
                }

            //current element is the max
            //look for the nearest min towards right
            }else if(A[i] == max){

                for(int j=i+1; j<n; j++){
                    if(A[j] == min){
                        minSize = Math.min(minSize, j-i+1);
                        break;
                    }
                }

            }

        }

        return minSize;

    }
}

```

### APPROACH 2 (Carry Foward Technique)

For any position, if it's a min value and we know the nearest max on the right, we can figure out the possible answer. Similarly, if that number is max and we know the nearest min on the right, we have a possible solution.

So, what we can do it, iterate from right to left and maintain the index of min and max value (which can be pre-calculated before iteration). Whenever we get a min or max value, we update its index. The other required min or max which is nearest on the right would already have been calculated in this way. We can use this to get the length and keep updating the final min length which will be the answer.

```java
public class Solution {
    
    public int solve(int[] A) {

        int n = A.length;
        
        //get the min and max from the array
        int min = Integer.MAX_VALUE;
        int max = Integer.MIN_VALUE;

        for(int i=0; i<n; i++){
            min = Math.min(min, A[i]);
            max = Math.max(max, A[i]);
        }

        //if both min and max are same, the min length has to be 1
        if(min == max) return 1;

        //init
        int minIndex = -1;
        int maxIndex = -1;

        //the min possible answer will be the length of the array
        int minLength = A.length;

        //iterate from right to left
        for(int i=n-1; i>=0; i--){
            
            //if the current element is min, update its index
            if(A[i] == min){
                minIndex = i;
            //if the current element is max, update its index
            }else if(A[i] == max){
                maxIndex = i;
            }
            //from the above code, we have the min/max index on the right of current position

            //if we have both min and max index, only then we can calculate the length
            if(minIndex != -1 && maxIndex != -1){
                //min...max or max...min, both ranges are possible
                //so we can take absolute value
                minLength = Math.min(minLength, Math.abs(minIndex-maxIndex)+1);
            }

        }

        return minLength;
    
    }

}
```
