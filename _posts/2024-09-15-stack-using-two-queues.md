---
layout: post
title: Stack using two queues (geeksforgeeks - SDE Sheet)
date: 2024-09-15 13:59 +0530
author: "Gaurav Kumar"
tags: "java stack queue geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Implement a Stack using two queues q1 and q2.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/stack-using-two-queues/1?page=7)

## SOLUTION

```java
class Queues
{
    Queue<Integer> q1 = new LinkedList<Integer>();
    Queue<Integer> q2 = new LinkedList<Integer>();

    //Function to push an element into stack using two queues.
    void push(int a)
    {

        // add element in q2
        q2.add(a);

        // move all elements from q1 to q2
        // so, the current element will become the first element to be remove, which is what we want as per stack logic
        while(!q1.isEmpty()){
            q2.add(q1.poll());
        }

        // swap q1 & q2
        // this helps in avoiding logic to check which queue will act as source or destination
        Queue<Integer> tq = q1;
        q1 = q2;
        q2 = tq;

    }

    //Function to pop an element from stack using two queues.
    int pop()
    {

        // while pushing, we have made sure that the final queue q1 has all the elements
        // if its empty, return -1
        if(q1.isEmpty())
            return -1;

        // otherwise get the element from queue which will be logically the top of stack
        return q1.poll();

    }

}
```
