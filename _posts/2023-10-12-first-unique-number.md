---
layout: post
title: First Unique Number
date: 2023-10-12 21:15 +0530
author: "Gaurav Kumar"
tags: "java queue leetcode leetcodealgo100"
categories: "queue"
---

## PROBLEM DESCRIPTION

You have a queue of integers, you need to retrieve the first unique integer in the queue.

Implement the FirstUnique class:

- FirstUnique(int[] nums) Initializes the object with the numbers in the queue.
- int showFirstUnique() returns the value of the first unique integer of the queue, and returns -1 if there is no such integer.
- void add(int value) insert value to the queue.

[leetcode](https://leetcode.com/problems/first-unique-number/)

## SOLUTION

Good Explanation: [Techdose](https://www.youtube.com/watch?v=x_m69OeOHN8)

````java
class FirstUnique {

    Queue<Integer> q;
    HashMap<Integer, Integer> map;

    public FirstUnique(int[] nums) {

        q = new LinkedList<>();
        map = new HashMap<>();

        // for each element, use hashmap to track the frequency based on the initial nums array
        for(int i=0; i<nums.length; i++){

            int x = nums[i];

            map.put(x, map.getOrDefault(x, 0)+1);
            q.add(x);

        }

    }

    public int showFirstUnique() {

        // starting from the head of the queue, keep checking if the current element has a frequency of more than 1 (not unique)
        // in that case, we can remove it
        // updating the frequency in the hashmap is not needed (we are not technically deleting the element)
        // the idea is to maintain the unique element at the head of the queue
        while(!q.isEmpty() && map.get(q.peek()) != 1){
            q.poll();
        }

        // if all the elements were removed, in which case the queue will be empty, return -1
        if(q.isEmpty()){
            return -1;
        // otherwise, the first element would have the frequency as 1
        }else{
            return q.peek();
        }

    }

    public void add(int value) {

        // if the current element was never added before, so it's unique (but may not be the FIRST unique)
        // it can potentially be an answer in the future, so we will need to track it
        // so add it to the queue and update hashmap with frequency 1
        if(!map.containsKey(value)){
            q.add(value);
            map.put(value, 1);

        // if the current element is already present, then it gets a bit interesting
        }else{

            // we can do this to update the frequency: map.put(value, map.get(value)+1); -> accepted answer
            // however, if the frequency was the current element was already more than 1, it doesn't really make any difference
            // so, we can update the frequency only when it's 1, to mark it as "no longer unique"
            // this is a small optimization which can help
            // also note that we don't need to track it since it cannot be unique. so we don't add it to the queue

            if(map.get(value) == 1){
                map.put(value, 2);
            }

        }

    }
}

/**
 * Your FirstUnique object will be instantiated and called as such:
 * FirstUnique obj = new FirstUnique(nums);
 * int param_1 = obj.showFirstUnique();
 * obj.add(value);
 */```
````
