---
layout: post
title: Java Multithreading - Producer/Consumer (Using Low-Level Synchronization)
date: 2023-07-12 23:42 +0530
author: "Gaurav Kumar"
tags: "java multithreading theory"
categories: "multithreading"
---

This following post is based on the Udemy Course:
[Java Multithreading](https://www.udemy.com/course/java-multithreading/learn/lecture/107238#content)

---

In the updated version of the application, the main app remains unchanged. It creates two threads: one for the producer and another for the consumer. However, instead of using a high-level data structure like a BlockingQueue, we will implement low-level synchronization using the wait() and notify() methods.

The producer thread will produce items and notify the consumer when an item is available. If the consumer thread tries to consume an item when none are available, it will wait until the producer notifies it.

This approach allows for more fine-grained control over synchronization but requires explicit handling of thread coordination. By using the wait() and notify() methods, we ensure that the consumer waits patiently until an item is produced, and the producer notifies the consumer when it has produced an item.

### Processor.java

```java
package com.threads;

import java.util.LinkedList;
import java.util.Random;

public class Processor {

 //FIFO
 LinkedList<Integer> list = new LinkedList<>();

 //we will maintain max 10 elements in the queue
 private static final int LIMIT = 10;
 
 //used for synchronization between the producer and consumer threads
 Object lock = new Object();
 
 public void produce() throws InterruptedException {
  
  Random random = new Random();
  
  int value = 0;
  
  //run forever
  while(true) {
   
   //get intrinsic lock on the "lock" object
   synchronized (lock) {
    
    //if the size has reached the limit, wait for notification
    while(list.size() == LIMIT) {
     lock.wait();
    }
    
    //code will reach here when size of the queue is below its limit
    //add element to the queue
    list.add(value);
    
    //increase the value to  be added next time
    value++;
    System.out.println("Producer added " + value + " to the list; ListSize: " + list.size());
    
    //notify the waiting thread
    //wakes up a single thread that is waiting on this object's monitor. 
    lock.notify();
    
   }
   
   //sleep for random amount of time between [0-49]
   Thread.sleep(random.nextInt(50));
   
  }
  
 }
  
 
 public void consume() throws InterruptedException {
  
  Random random = new Random();
  
  //run forever
  while(true) {
   
   //get the intrinsic lock on the "lock" object
   synchronized (lock) {
    
    //if the size of the queue is 0, wait for notification
    while(list.size() == 0) {
     lock.wait();
    }
    
    //we can reach here only when list size is not 0
    //remove the first element from the queue
    int value = list.removeFirst();
    
    System.out.println("Consumed: " + value + "; LisSize: " + list.size());
   
    //notify the other thread
    //wakes up a single thread that is waiting on this object's monitor. 
    lock.notify();
    
   }
   
   //sleep for random amount of time between [0-59]
   Thread.sleep(random.nextInt(60));
   
  }
 }
 
}
```

### App.java

```java
package com.threads;

public class App {
 
 public static void main(String[] args){
   
  Processor processor = new Processor();
  
  //producer thread
  Thread t1 = new Thread(new Runnable() {
   
   @Override
   public void run() {
    try {
     processor.produce();
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
     processor.consume();
    } catch (InterruptedException e) {
     e.printStackTrace();
    }
   }
  });
  
  t1.start();
  t2.start();
  
  try {
   t1.join();
   t2.join();
  } catch (InterruptedException e) {
   e.printStackTrace();
  }
  
 }

}
```
