---
layout: post
title: Java Multithreading - Producer/Consumer
date: 2023-07-11 22:49 +0530
author: "Gaurav Kumar"
tags: "java multithreading theory"
categories: "multithreading"
---

This following post is based on the Udemy Course:
[Java Multithreading](https://www.udemy.com/course/java-multithreading/learn/lecture/107238#content)

---

### Blocking Queue (thread-safe queue implementation)

A Queue that additionally supports operations that wait for the queue to become non-empty when retrieving an element, and wait for space to become available in the queue when storing an element.  

When retrieving an element from a BlockingQueue, the take() method is used, which blocks the calling thread until an element becomes available in the queue. If the queue is empty, the thread will remain blocked until another thread inserts an item.  

When storing an element in a BlockingQueue, the put() method is used. If the queue is full, the put() method will block the calling thread until space becomes available in the queue, either by other threads dequeuing elements or by clearing the queue completely.  

[Oracle Doc](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/BlockingQueue.html)

### Summary of BlockingQueue methods

BlockingQueue methods come in four forms, with different ways of handling operations that cannot be satisfied immediately, but may be satisfied at some point in the future: one throws an exception, the second returns a special value (either null or false, depending on the operation), the third blocks the current thread indefinitely until the operation can succeed, and the fourth blocks for only a given maximum time limit before giving up.  

| Operation | Throws exception | Special value | Blocks | Times out             |
|-----------|------------------|---------------|--------|-----------------------|
| Insert    | add(e)           | offer(e)      | put(e) | offer(e, time, unit)  |
| Remove    | remove()         | poll()        | take() | poll(time, unit)      |
| Examine   | element()        | peek()        | N/A    | N/A                   |

### Producer / Consumer with Blocking Queue

```java
package com.threads;

import java.util.Random;
import java.util.concurrent.ArrayBlockingQueue;
import java.util.concurrent.BlockingQueue;

public class App {
 
 private static BlockingQueue<Integer> blockingQueue = new ArrayBlockingQueue<>(10);
 
 public static void main(String[] args) throws InterruptedException {
  
  //producer thread
  Thread t1 = new Thread(new Runnable() {
   
   @Override
   public void run() {
    try {
     producer();
    } catch (InterruptedException e) {
     e.printStackTrace();
    }
   }
  });
  
  //consumer thread
  Thread t2 = new Thread(new Runnable() {
   
   @Override
   public void run() {
    try {
     consumer();
    } catch (InterruptedException e) {
     e.printStackTrace();
    }
   }
  });
  
  //both these threads have a while loop so they will continue running indefinitely
  t1.start();
  t2.start();
  
  //waits for this thread to die. 
  t1.join();
  t2.join();
  
 }
 
 //sleep for random amount of time between 0-200 miliseconds and then add an item to the blocking queue
 private static void producer() throws InterruptedException {
  
  Random random = new Random();
  
  while(true) {

   //blockingQueue.put can cause InterruptedException
   Thread.sleep(random.nextInt(200));
   
   blockingQueue.put(random.nextInt(100));
  }
  
 }

 //sleep for random amount of time between 0-50 miliseconds and then take an item from the blocking queue
 private static void consumer() throws InterruptedException {
  
  Random random = new Random();
  
  while(true) {
   
   Thread.sleep(random.nextInt(50));
   
   if(random.nextInt(5) == 0) {

    Integer val = blockingQueue.take();
    System.out.println("Taken: " + val + "; Queue Size: " + blockingQueue.size());

   }
   
  }
  
 }
}

```

SAMPLE OUTPUT:

```text
Taken: 11; Queue Size: 0
Taken: 15; Queue Size: 0
Taken: 63; Queue Size: 1
Taken: 25; Queue Size: 0
Taken: 6; Queue Size: 0
Taken: 49; Queue Size: 0
Taken: 41; Queue Size: 3
Taken: 95; Queue Size: 2
Taken: 53; Queue Size: 2
Taken: 79; Queue Size: 3
Taken: 85; Queue Size: 2
Taken: 71; Queue Size: 4
Taken: 76; Queue Size: 3
Taken: 39; Queue Size: 3
Taken: 82; Queue Size: 2
Taken: 73; Queue Size: 1
Taken: 85; Queue Size: 0
Taken: 50; Queue Size: 2
Taken: 13; Queue Size: 3
Taken: 21; Queue Size: 6
Taken: 93; Queue Size: 5
Taken: 57; Queue Size: 5
Taken: 2; Queue Size: 4
Taken: 55; Queue Size: 5
Taken: 31; Queue Size: 4
Taken: 71; Queue Size: 4
Taken: 94; Queue Size: 7
Taken: 45; Queue Size: 7
Taken: 37; Queue Size: 7
Taken: 6; Queue Size: 8
Taken: 48; Queue Size: 9
Taken: 61; Queue Size: 9
Taken: 60; Queue Size: 9
Taken: 63; Queue Size: 9
Taken: 14; Queue Size: 9
Taken: 59; Queue Size: 9
Taken: 46; Queue Size: 8
Taken: 28; Queue Size: 7
Taken: 2; Queue Size: 6
Taken: 4; Queue Size: 5
```
