---
layout: post
title: Josephus Problem
date: 2023-04-26 23:10 +0530
tags: "java arrays geeksforgeeks"
categories: "arrays"
---

### PROBLEM DESCRIPTION

Given the total number of persons n and a number k which indicates that k-1 persons are skipped and kth person is killed in circle in a fixed direction.
After each operation, the count will start from k+1th person. The task is to choose the safe place in the circle so that when you perform these operations starting from 1st place in the circle, you are the last one remaining and survive.

[geeksforgeeks](https://practice.geeksforgeeks.org/problems/josephus-problem/1?utm_source=gfg&utm_medium=article&utm_campaign=bottom_sticky_on_article)

### SOLUTION

When k=2, refer to the below explanation:

![snapshot]({{ site.baseurl }}/assets/img/josephus.png)

When k more than 2, we will need to make use of recursion. Good Explanation: [Anuj YouTube](https://www.youtube.com/watch?v=46zD5d9y9b4)

```java
class Solution
{
   public int josephus(int n, int k)
    {
        return 1 + helper(n, k);
    }
    
    public int helper(int n, int k){
        if(n == 1) return 0;
        return (helper(n-1, k) + k)%n;
    }

}
```
