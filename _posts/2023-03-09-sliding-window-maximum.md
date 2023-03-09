---
layout: post
title: Sliding Window Maximum
date: 2023-03-09 21:41 +0530
author: "Gaurav Kumar"
tags: "java leetcode sliding_window neetcode150 important"
categories: "neetcode150"
---

### Problem Description

You are given an array of integers nums, there is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position.

Return the max sliding window.

[leetcode](https://leetcode.com/problems/sliding-window-maximum/description/)

### Solution

[NeetCode YouTube](https://www.youtube.com/watch?v=DfljaUwZsOk)

```java
class Solution {

    public int[] maxSlidingWindow(int[] nums, int k) {
        
        int n = nums.length;

        //we will use dequeue to add elements to the last and remove from first, while maintaining the largest number in the beginning
        Deque<Integer> q = new LinkedList<>(); //we will store the index in this queue

        //there will be (n-k+1) windows
        int[] ans = new int[n-k+1];

        //insert first k numbers to the queue
        for(int i=0; i<k; i++){
            
            //if there is a larger number, any number which came before that has no significance
            //so keep removing the number from the queue in that case
            while(!q.isEmpty() && nums[i] >= nums[q.getLast()]){
                q.removeLast();
            }

            //finally add the current number's index
            //we are using an index so that later, we can use this to identify if we are still within the windows range
            q.addLast(i);
        }

        //first max will be at the beginning of the queue
        ans[0] = nums[q.getFirst()];

        //we will use this idx index to add to our answer array
        int idx=1;  

        //loop through other windows which will start from index k
        for(int i=k; i<n; i++){
            
            //current index
            int current = i;

            //keep removing items from the queue while the number at current index is more than what is currently there in the queue
            //remember, we have stored index in the queue, so we need to check the actual number using nums[index]
            while(!q.isEmpty() && nums[current] >= nums[q.getLast()]){
                q.removeLast();
            }

            //at this point, it's possible that the left most number (which is largest) was part of the earlier window
            //we can identify that by comparing the index
            //basically, if the left most index in the queue is out of the window (if it's less than the left side of the window which is i-k) then remove that
            if(!q.isEmpty() && q.getFirst() <= i-k){
                q.removeFirst();
            }

            //add the current index to the queue
            q.addLast(current);

            //next max will be number at left most index of the queue
            ans[idx++] = nums[q.getFirst()];

        }

        return ans;
        
    }

}
```

### Another way to code (Similar Solution)

```java
class Solution {

    public int[] maxSlidingWindow(int[] nums, int k) {
        
        int n = nums.length;

        Deque<Integer> q = new LinkedList<>(); //we will store the index in this queue

        int[] ans = new int[n-k+1];

        int l=0;
        int r=0;

        int idx=0;
        while(r<n){
            
            //while the current element is greater, keep remove elements from the queue from the right
            while(!q.isEmpty() && nums[q.getLast()] < nums[r]){
                q.removeLast();
            }

            //if the current largest element is actually part of previous window, remove it
            if(!q.isEmpty() && q.getFirst() < l){
                q.removeFirst();
            }

            //add the current index
            q.addLast(r);

            //if the window size is k, only then add to the answer array and increase the left of the window
            if((r+1) >= k){
                ans[idx++] = nums[q.getFirst()];
                l++;
            }

            //increase the right of the window
            r++;
        }

        return ans;
        
    }

}
```

### Using Max Heap

```java
class Solution {
    
    public int[] maxSlidingWindow(int[] nums, int k) {

        int n = nums.length;

        //max heap based on value of pair
        PriorityQueue<Pair> pq = new PriorityQueue<>( (p1, p2) -> p2.value - p1.value);

        //insert first k numbers
        for(int i=0; i<k; i++) 
            pq.add(new Pair(nums[i], i));

        //there will be [n-k+1] windows
        int[] ans = new int[n-k+1];

        //index to add to answer array
        int idx=0;

        //first one will be the top of max heap
        ans[idx++] = pq.peek().value;

        //add other elements one by one
        for(int i=k; i<n; i++){
            
            //add current element to the heap
            pq.add(new Pair(nums[i], i));

            //remove elements which are out of the window
            //it's possible that the max belongs to previous windows
            //we can check this by checking if the index of current max is less than the left side of the window (which will be i-k)
            while(pq.peek().index <= i-k){
                pq.poll();
            }

            //once the elements which are out of window are removed, we will have the largest element at the top which is our next max
            ans[idx++] = pq.peek().value;   

        }

        return ans;

    }

}

class Pair{
    
    int value;
    int index;

    Pair(int v, int i){
        value = v;
        index = i;
    }

    public String toString(){
        return String.valueOf(value);
    }

}
```
