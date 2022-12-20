---
layout: post
title: Find the Duplicate Number
date: 2022-12-20 18:47 +0530
author: "Gaurav Kumar"
tags: "java leetcode linkedlist"
categories: "linkedlist"
---

### PROBLEM DESCRIPTION

Given an array of integers nums containing n + 1 integers where each integer is in the range [1, n] inclusive.

There is only one repeated number in nums, return this repeated number.

You must solve the problem without modifying the array nums and uses only constant extra space.

[leetcode](https://leetcode.com/problems/find-the-duplicate-number/description/)

### SOLUTION

### APPROACH 1

The simplest way using O(N) space is by using a HashSet. Keep adding numbers in the list to the HashSet and keep a check if it was already present. If it was already present, that number must be the duplicate.

### APPROACH 2

One observation is that the range of numbers is [1-N]. So, for each number, we can make the number at index (i-1) to negative. This will mark that the number has been "visited". When we encounter the duplicate number, the corresponding index will already have a negative number. So, we can return the current number as the duplicate.

```java
class Solution {

    public int findDuplicate(int[] nums) {

        for(int i=0; i<nums.length; i++){
            
            int x = Math.abs(nums[i]);

            if(nums[x-1] < 0) return x;

            nums[x-1] = nums[x-1] * -1;

        }

        //restore
        for(int i=0; i<nums.length; i++){
            nums[i] = Math.abs(nums[i]);
        }

        return 0;
        
    }

}
```

### APPROACH 3: USING BINARY SEARCH

> Example: [4,3,4,5,2,4,1]

Since the numbers are in the range of [1-N], where N+1 is the size of the array, we have one important observation:

- If we pick 3, there are exactly 3 numbers less than or equal to 3.
- If we pick 4, there are more than 4 numbers which are less than or equal to 4.
- If we pick any number after 4, it will be more than that number

So, if we consider our search space as [1-N], we can apply binary search and look for possible answers based on the above condition. If the number of numbers which are >= X is equal to X, then it must not be a duplicate. Otherwise, that is a possible answer, look for better answer which are lower. The lowest number which satisfies this condition will be our answer.

```java
class Solution {
    
    public int findDuplicate(int[] nums) {

        int l=1;
        int r=nums.length-1;

        int ans = 0;

        while(l<=r){
            
            int m = (l+r)/2;

            if(check(m, nums)){
                ans = m;
                r=m-1;
            }else{
                l=m+1;
            }

        }

        return ans;
        
    }

    //how many numbers in nums array are >= m
    public boolean check(int m, int[] nums){
        
        int count = 0;

        for(int i=0; i<nums.length; i++) if(nums[i] <= m) count++;

        if(count > m) return true;

        return false;
    }

}
```

### APPROACH 4

This problem can be converted into: Find the start of the cycle in a LinkedList.  
[Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/description/)

```java
class Solution {

    public int findDuplicate(int[] nums) {

        int slow = nums[0];
        int fast = nums[0];

        do{
            slow = nums[slow];
            fast = nums[nums[fast]]; //move two steps ahead
        }while(slow != fast);

        slow = nums[0];

        while(slow != fast){
            slow = nums[slow];
            fast = nums[fast];
        }
        
        return slow; //or fast

    }
    
}
```
