---
layout: post
title: Java Multithreading - The Synchronized Keyword
date: 2023-07-04 20:35 +0530
author: "Gaurav Kumar"
tags: "java multithreading theory"
categories: "multithreading"
---

This following post is based on the Udemy Course:
[Java Multithreading](https://www.udemy.com/course/java-multithreading/learn/lecture/107238#content)

---

In the following code, the result of sysout(count) will be 0. This is because, even though we have started the two threads t1 and t2, we are printing the value of "count" before these threads can update the count variable.

```java
package demo;

public class App {
 
 private int count = 0;
 

 public static void main(String[] args) {

  App app = new App();
  app.doWork();
  
 }
 
 public void doWork() {
  
  Thread t1 = new Thread(new Runnable() {
   
   @Override
   public void run() {
    
    for(int i=0; i<10000; i++) {
     count++;
    }
    
   }
  });
  
  Thread t2 = new Thread(new Runnable() {
   
   @Override
   public void run() {
    
    for(int i=0; i<10000; i++) {
     count++;
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
  
  
  System.out.println(count);

 }

}
```

To wait for these threads to complete, we can use the join() method.
But now, the value of count is not guaranteed to be 20000, even though each thread t1 and t2 have incremented the value of count by 10,000!

Let us try to understand why this is happening.  

We must understand that the two threads t1 and t2 are concurrently running. The count++ operation is equivalent of: ```count = count + 1```. This involves majorly the following steps:  

- Get the current value of count
- Add 1 to it
- Store the updated value to count variable

Let's say, t1 got the current value and it is trying to add 1 to it. Before even it completes the 3rd step, the second thread t2 was able to do the same operation 10 or more times (all 3 steps multiple times). After that, interestingly, t1 will complete its last step which would bring the count to a lower old value.  

This problem is because we are allowing the shared variable to be accessed and modified at the same time by two different threads.  

```java
package demo;

public class App {
 
 private int count = 0;
 

 public static void main(String[] args) {

  App app = new App();
  app.doWork();
  
 }
 
 public void doWork() {
  
  Thread t1 = new Thread(new Runnable() {
   
   @Override
   public void run() {
    
    for(int i=0; i<10000; i++) {
     count++;
    }
    
   }
  });
  
  Thread t2 = new Thread(new Runnable() {
   
   @Override
   public void run() {
    
    for(int i=0; i<10000; i++) {
     count++;
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
  
  
  System.out.println(count);

 }

}
```

What's the solution? We can use __synchorized__ method to update count.  

Every object in Java has an intrisic monitor/lock/mutex. If we try to call a synchroized method of an object, we have to acquire an intrinsic lock before we can call it. Only one thread can acquire an intrinsic lock at a time. If another thread tries to call the same method, it will have to just wait until the previous thread releases the intrinsic lock.  

We don't need to declare count as volatile. Because if we are running something in the synchroized block then it is guranteed that the current state of the variable will be visible to all the threads.

```java
package demo;

public class App {
 
 private int count = 0;
 
 //create a synchrozied method to update count value
 public synchronized void increment() {
  count++;
 }

 public static void main(String[] args) {

  App app = new App();
  app.doWork();
  
 }
 
 public void doWork() {
  
  //first thread
  Thread t1 = new Thread(new Runnable() {
   
   @Override
   public void run() {
    
    for(int i=0; i<10000; i++) {
     increment();
    }
    
   }
  });
  
  //second thread
  Thread t2 = new Thread(new Runnable() {
   
   @Override
   public void run() {
    
    for(int i=0; i<10000; i++) {
     increment();
    }
    
   }
  });
  
  //start the threads
  t1.start();
  t2.start();
  
  //wait for both the threads to complete
  try {
   t1.join();
   t2.join();
  } catch (InterruptedException e) {
   e.printStackTrace();
  }
  
  //check count
  System.out.println(count);

 }

}
```
