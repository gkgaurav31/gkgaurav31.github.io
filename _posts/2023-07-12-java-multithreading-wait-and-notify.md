---
layout: post
title: Java Multithreading - Wait and Notify
date: 2023-07-12 21:03 +0530
author: "Gaurav Kumar"
tags: "java multithreading theory"
categories: "multithreading"
---

This following post is based on the Udemy Course:
[Java Multithreading](https://www.udemy.com/course/java-multithreading/learn/lecture/107238#content)

---

### wait()

The wait() method is a part of java.lang.Object class. When wait() method is called, the calling thread stops its execution until notify() or notifyAll() method is invoked by some other Thread. The wait() method has 3 variations:  

- wait(): This is a basic version of the wait() method which does not take any argument. It will cause the thread to wait till notify is called.
- wait(long timeout): This version of the wait() method takes a single timeout argument. It will cause the thread to wait either till notify is called or till timeout (One which occurs earlier).
- wait(long timeout, int nanoseconds): This version of the wait() method takes a timeout argument as well as a nanosecond argument for extra precision.

### notify()

The notify() method is defined in the Object class, which is Java’s top-level class. It’s used to wake up only one thread that’s waiting for an object, and that thread then begins execution. The thread class notify() method is used to wake up a __single thread__.

[Oracle Doc](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/Object.html#wait())

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

### Processor.java

```java
package com.threads;

import java.util.Scanner;

public class Processor {

 public void produce() throws InterruptedException {
  
  //producer thread will get the lock and run this method
  synchronized (this) {
   System.out.println("Producer running.");
   
   //causes the current thread to wait until it is awakened, typically by being notified or interrupted. 
   //this of it as relinquishing the intrinsic lock and waiting for a notification to get the lock again
   wait();
   
   //continue, once it gets the lock again
   System.out.println("Resumed");
  }
  
 }
 
 public void consume() throws InterruptedException {
  
  //we have added a sleep to ensure that producer thread gets the intrinsic lock on the current object first
  Thread.sleep(2000);
  
  Scanner scanner = new Scanner(System.in);
  
  //the consumer thread will get the lock once the producer thread calls "wait"
  synchronized (this) {
   
   //wait for return key in the input
   System.out.println("waiting for return");
   scanner.nextLine();
   
   //Wakes up a single thread that is waiting on this object'smonitor. 
   //If any threads are waiting on this object, one of them is chosen to be awakened. 
   //The choice is arbitrary and occurs at the discretion of the implementation. 
   //A thread waits on an object's monitor by calling one of the wait methods. 
   //The awakened thread will not be able to proceed until the current thread relinquishes the lock on this object. 
   //The awakened thread will compete in the usual manner with any other threads that might be actively competing to synchronize on this object; 
   //for example, the awakened thread enjoys no reliable privilege or disadvantage in being the next thread to lock this object. 
   notify();
   
   //even after calling notify, the producer thread can get the intrinsic lock only after this thread completes
   System.out.println("sleeping for 5 seconds");
   Thread.sleep(5000);
  }
  
 }
 
}
```
