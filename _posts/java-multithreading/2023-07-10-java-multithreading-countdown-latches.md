---
layout: post
title: Java Multithreading - Countdown Latches
date: 2023-07-10 20:59 +0530
author: "Gaurav Kumar"
tags: "java multithreading theory"
categories: "multithreading"
---

This following post is based on the Udemy Course:
[Java Multithreading](https://www.udemy.com/course/java-multithreading/learn/lecture/107238#content)

---

> A synchronization aid that allows one or more threads to wait until a set of operations being performed in other threads completes.
> A CountDownLatch is initialized with a given count.The await methods block until the current count reaches zero due to invocations of the countDown method, after which all waiting threads are released and any subsequent invocations of await return immediately. This is a one-shot phenomenon-- the count cannot be reset. If you need a version that resets the count, consider using a CyclicBarrier.
> A CountDownLatch is a versatile synchronization tool and can be used for a number of purposes. A CountDownLatch initialized with a count of one serves as asimple on/off latch, or gate: all threads invoking await wait at the gate until it is opened by a thread invoking countDown. A CountDownLatch initialized to N can be used to make one thread wait until N threads have completed some action, or some action has been completed N times.
> A useful property of a CountDownLatch is that it doesn't require that threads calling countDown wait for the count to reach zero before proceeding, it simply prevents anythread from proceeding past an await until allthreads could pass.

```java
package com.threads;

import java.util.concurrent.CountDownLatch;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

class Processor implements Runnable{

 CountDownLatch latch;
 
 Processor(CountDownLatch latch){
  this.latch = latch;
 }
 
 @Override
 public void run() {
  
  System.out.println("Started.");
  
  try {
   Thread.sleep(3000);
  } catch (InterruptedException e) {
   e.printStackTrace();
  }
  
  //reduce the count of latch by 1
  latch.countDown();
  
 }
 
}

public class App {
 
 public static void main(String[] args) {
  
  //create a latch
  CountDownLatch countDownLatch = new CountDownLatch(3);
  
  //use an executor service to create 3 worker threads
  ExecutorService executorService = Executors.newFixedThreadPool(3);
  
  //assign 3 tasks to the threads
  //the threads will reduce the value of latch by 1
  for(int i=0; i<3; i++) {
   executorService.submit(new Processor(countDownLatch));
  }
  
  //wait until the value of latch becomes 0
  try {
   countDownLatch.await();
  } catch (InterruptedException e) {
   e.printStackTrace();
  }
  
  System.out.println(Thread.currentThread().getName() + " has finished");
  
 }

}
```

OUTPUT:

```text
Started.
Started.
Started.
main has finished
```
