---
layout: post
title: Design Bounded Blocking Queue
date: 2024-08-04 22:18 +0530
author: "Gaurav Kumar"
tags: "java design queue leetcode "
categories: "design"
---

## Problem Description

Implement a thread-safe bounded blocking queue that has the following methods:

- BoundedBlockingQueue(int capacity) The constructor initializes the queue with a maximum capacity.
- void enqueue(int element) Adds an element to the front of the queue. If the queue is full, the calling thread is blocked until the queue is no longer full.
- int dequeue() Returns the element at the rear of the queue and removes it. If the queue is empty, the calling thread is blocked until the queue is no longer empty.
- int size() Returns the number of elements currently in the queue.

Your implementation will be tested using multiple threads at the same time. Each thread will either be a producer thread that only makes calls to the enqueue method or a consumer thread that only makes calls to the dequeue method. The size method will be called after every test case.

Please do not use built-in implementations of bounded blocking queue as this will not be accepted in an interview.

[leetcode](https://leetcode.com/problems/design-bounded-blocking-queue/description/)

## Solution

```java
class BoundedBlockingQueue {

    // Queue to store elements.
    private Queue<Integer> q;

    // Maximum capacity of the queue.
    private int maxCapacity;

    public BoundedBlockingQueue(int capacity) {
        q = new LinkedList<>();
        maxCapacity = capacity;
    }

    // Method to add an element to the queue.
    // It waits if the queue is full.
    public synchronized void enqueue(int element) throws InterruptedException {

        // Wait while the queue is at its maximum capacity.
        while(q.size() == maxCapacity)
            wait();

        // Add the element to the queue.
        q.add(element);

        // Notify all waiting threads that an element has been added.
        notifyAll();
    }

    // Method to remove and return an element from the queue.
    // It waits if the queue is empty.
    public synchronized int dequeue() throws InterruptedException {

        // Wait while the queue is empty.
        while(q.size() == 0)
            wait();

        // Notify all waiting threads that an element has been removed.
        notifyAll();

        // Remove and return the element from the queue.
        return q.poll();
    }

    // Method to get the current size of the queue.
    public int size() {
        return q.size();
    }
}
```
