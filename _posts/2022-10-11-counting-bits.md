---
layout: post
title: Counting Bits
date: 2022-10-11 14:19 +0530
author: "Gaurav Kumar"
tags: "java neetcode150"
categories: "neetcode150"
---

This question is part of [NeetCode150 series](https://neetcode.io/practice).  

## Problem Description

Given an integer n, return an array ans of length n + 1 such that for each i (0 <= i <= n), ans[i] is the number of 1's in the binary representation of i.  
[leetcode](https://leetcode.com/problems/counting-bits/)

### Solution

### APPROACH 1

For each number [0,N], we can make use of the code we used in this problem: [Count Number of 1 bits]({% post_url 2022-10-11-number-of-1-bits %})  

Time Complexty: O(NlogN)

```java
class Solution {
    public int[] countBits(int n) {
    
        int[] ans = new int[n+1];
        
        for(int i=0; i<=n; i++){
            ans[i] = hammingWeight(i);
        }
        
        return ans;
        
    }
    
    public int hammingWeight(int n) {
        int c=0;
        while(n!=0){
            if( (n&1) == 1) c++;
            n=(n>>>1);
        }
        return c;
    }
    
}
```

### APPROACH 2

Let's say, we have an even number. The right most bit is 0 for that number. So, if we do a right shift, the number of 1s is still going to be the same. Right shift once, is equivalent of dividing the number by 2. This means, number of 1s in that even number is same as the number of 1s for N/2.  

For odd number, the right most bit is 1. If we do a right shift (divide by 2), we will lose that 1 bit which is set.  

We can use these facts to determine our final answer, by dividing the number by 2. Each time check if the current number is odd or even. If the number is even, simply divide it by 2. If it is odd, we increase the count by 1 and divide the number again. We can keep doing this until N equals 0.

```java
class Solution {
    
    public int[] countBits(int n) {
    
        int[] ans = new int[n+1];
        
        for(int i=0; i<=n; i++){
            
            ans[i] = count(ans, i);
            
        }
        
        return ans;
        
    }
    
    public int count(int[] arr, int n){
        
        int count=0;
        
        while(n!=0){
            if(n%2 == 0){
                n = n/2;
            }else{
                count++;
                n = n/2;
            }
        }
        
        return count;
        
    }
    
    
}
```

### APPROACH 3

Using [Brian Kernighan's Algorithm](https://iq.opengenus.org/brian-kernighan-algorithm/).  

> The main idea behind this algorithm is that when we subtract one from any number, it inverts all the bits after the rightmost set bit i.e. it turns 1 to 0 and 0 to 1. (including the right most set bit)

Example:  

|Number||
|10|0|1|0|1|0|
|9|0|1|0|__0__|__1__|
|8|0|1|0|0|__0__|
|7|0|__0__|__1__|__1__|__1__|

:star2: The magic is that, everytime we do a & with N and (N-1), we can say that the right most set bit will become 0. If there are X set bit, then our loop will run just X number of times!  

![snapshot]({{ site.baseurl }}/assets/img/count_set_bits.png)

```java
class Solution {
    
    public int[] countBits(int n) {
    
        int[] ans = new int[n+1];
        
        for(int i=0; i<=n; i++){
            
            ans[i] = count(ans, i);
            
        }
        
        return ans;
        
    }
    
    //Brian Kernighan's Algorithm
    public int count(int[] arr, int n){
        
        int count=0;
        
        while(n!=0){
            n = n&(n-1);
            count++;
        }
        
        return count;
        
    }
    
    
}
```

### ANOTHER WAY TO CODE

We can further optimize the above approach by making use of previous calculated result. The idea is - Let's say I have any number N. I can unset the right most set bit by doiing: N&(N-1). So, N will have 1 more set bit than the number of set bits of N&(N-1).  

Initialize ans[0]=0. We start iterating from 1. We can the number of set bits for N&(N-1) and add 1 to it. We keep doing this for subsequent numbers.  

```java
class Solution {
    public int[] countBits(int n) {
        int[] ans = new int[n+1];
        ans[0] = 0;
        for(int i=1; i<=n; i++){
            ans[i] = ans[i&(i-1)] + 1;
        }
        
        return ans;
        
    }  
}
```
