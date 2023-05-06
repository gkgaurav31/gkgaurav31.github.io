---
layout: post
title: Count pairs in array divisible by K
date: 2023-05-06 23:42 +0530
author: "Gaurav Kumar"
tags: "java modulo geeksforgeeks important"
categories: " modulo"
---

### PROBLEM DESCRIPTION

You are given a number N. Find the total number of setbits in the numbers from 1 to N.

[geeksforgeeks](https://www.geeksforgeeks.org/count-total-set-bits-in-all-numbers-from-1-to-n/)

### SOLUTION

![snapshot]({{ site.baseurl }}/assets/img/count-pairs-in-array-divisible-by-k.png)

```java
class Solution
{
    public static long countKdivPairs(int nums[], int n, int k)
    {

        //to store the count of numbers for remainder [0,k-1]
        Map<Long, Long> map = new HashMap<>();

        //init, remainder count
        for(int i=0; i<n; i++){
            long remainder = nums[i]%k;
            map.put(remainder, map.getOrDefault(remainder, 0L)+1);
        }

        //init answer
        long total=0;

        //if there are elements which has 0 as the remainder, we can pair them together in nC2 ways
        if(map.containsKey(0L)){
            total = pickTwoWays(map.get(0L));
        }
       
        //other pairs which can be formed using remainder (i) and (k-i)
        long i=1;
        long j=k-1;
        
        while(i<j){
            
            if(map.containsKey(i) && map.containsKey(j)){
                total = total + (map.get(i) * map.get(j));
            }
            
            i++;
            j--;
            
        }
        
        //if k is even, we can form more pairs using elements which have the remainder as k/2
        long mid = k/2;
        if(k%2 == 0 && map.containsKey(mid)){
            total = total + pickTwoWays(map.get(mid));
        }

        return total;
    }
    
    //number of ways to pick 2 elements from n elements = nC2
    public static long pickTwoWays(long n){
        return (n*(n-1))/2;
    }
    
}
```
