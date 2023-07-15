---
layout: post
title: Java Multithreading - Re-entrant Locks
date: 2023-07-13 20:22 +0530
author: "Gaurav Kumar"
tags: "java multithreading theory"
categories: "multithreading"
---

This following post is based on the Udemy Course:
[Java Multithreading](https://www.udemy.com/course/java-multithreading/learn/lecture/107238#content)

---

## USING REENTRANT LOCK: THE WRONG WAY

### App.java

We create two threads which will run the firstThread() and secondThread() methods of the Runner class. They will call the increment method to increase the value of count 1000 times. So, the expected total is 2000. This would give us the correct output. However, this approach is not recommended because the code after calling ```reentrantLock.lock();``` could lead to an exception, in which case the lock will never be release and the app will be stuck.

```java
package com.threads;

public class App {

 public static void main(String[] args) throws InterruptedException {
  
  Runner runner = new Runner();
  
  Thread t1 = new Thread(new Runnable() {
   
   @Override
   public void run() {
    runner.firstThread();
   }
  });
  
  Thread t2 = new Thread(new Runnable() {
   
   @Override
   public void run() {
    runner.secondThread();
   }
  });
  
  t1.start();
  t2.start();
  
  t1.join();
  t2.join();
  
  runner.finished();
  
 }
 
}
```

### Runner.java

```java
package com.threads;

import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class Runner {

 private int count = 0;
 private Lock reentrantLock = new ReentrantLock();
 
 public void increment() {
  
  for(int i=0; i<1000; i++) {
   count++;
  }
  
 }
 
 public void firstThread() {
  
  reentrantLock.lock();
  increment(); //problematic: if increment fails for some reason, the lock will never be released
  reentrantLock.unlock();
  
 }
 
 public void secondThread() {
  
  reentrantLock.lock();
  increment(); //problematic: if increment fails for some reason, the lock will never be released
  reentrantLock.unlock();
  
 }
 
 public void finished() {
  System.out.println("Value of count: " + count);
 }
 
}
```

## USING REENTRANT LOCK: BETTER WAY

Finally block will always execute, regardless of whether an exception is caught or not. This behavior ensures that any necessary cleanup or resource release operations are performed, regardless of the program's flow.

```java
try {
    // Code that may throw an exception
} catch (Exception e) {
    // Exception handling code
} finally {
    // Code that will always run
}
```

```java
package com.threads;

import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class Runner {

 private int count = 0;
 private Lock reentrantLock = new ReentrantLock();
 
 public void increment() {
  
  for(int i=0; i<1000; i++) {
   count++;
  }
  
 }
 
 public void firstThread() {
  
  reentrantLock.lock();

  try {
   increment();  
  } finally {
   reentrantLock.unlock();
  }
  
 }
 
 public void secondThread() {
  
  reentrantLock.lock();

  try {
   increment();  
  } finally {
   reentrantLock.unlock();
  }
  
 }
 
 public void finished() {
  System.out.println("Value of count: " + count);
 }
 
}
```

## AWAIT AND SIGNAL WITH REENTRANT LOCK

This behaves similar to wait() and notify() methods. These are part of the Condition class. We can get the object of the corresponding condition class using ```reentrantLock.newCondition();```. To call await() and signal() method, we can use this object. For example: ```condition.await();```, ```condition.signal();```. The behavior is similar to wait() and notify().

> Condition factors out the Object monitormethods (wait, notifyand notifyAll) into distinct objects togive the effect of having multiple wait-sets per object, by combining them with the use of arbitrary Lock implementations. Where a Lock replaces the use of synchronized methodsand statements, a Condition replaces the use of the Object monitor methods.

[Oracle Doc](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/concurrent/locks/Condition.html)

### await()

Causes the current thread to wait until it is signalled or interrupted. Before this method can return the current thread must re-acquire the lock associated with this condition. When the thread returns it is guaranteed to hold this lock.

### signal()

Wakes up one waiting thread.
If any threads are waiting on this condition then one is selected for waking up. That thread must then re-acquire the lock before returning from await.

```java
package com.threads;

import java.util.Scanner;
import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class Runner {

 private int count = 0;
 private Lock reentrantLock = new ReentrantLock();
 
 private Condition condition = reentrantLock.newCondition();
 
 public void increment() {
  
  for(int i=0; i<1000; i++) {
   count++;
  }
  
 }
 
 public void firstThread() throws InterruptedException {
  
  reentrantLock.lock();
  
  //we can only call this after we get the lock obviously
  //once called, it will give up the lock and wait for a signal to continue (it works similar to the wait() method)
  condition.await(); 
  
  System.out.println("Thread 1: Woken up.");

  //this will be executed only after the previous thread has completed the unlock
  try {
   increment();  
  } finally {
   reentrantLock.unlock();
  }
  
 }
 
 public void secondThread() throws InterruptedException {
  
  //adding sleep to ensure that firstThread get the lock first
  Thread.sleep(1000); 
  
  //get the lock once the first thread calls await
  reentrantLock.lock();
  
  System.out.println("Press the return key to continue.");
  new Scanner(System.in).nextLine();
  
  System.out.println("Got the return key.");
  
  condition.signal();

  try {
   increment();  
  } finally {
   //if we remove this, firstThead will never get executed. Although we have signalled it, however, we have not released the lock
   reentrantLock.unlock();
  }
  
 }
 
 public void finished() {
  System.out.println("Value of count: " + count);
 }
 
}
```
