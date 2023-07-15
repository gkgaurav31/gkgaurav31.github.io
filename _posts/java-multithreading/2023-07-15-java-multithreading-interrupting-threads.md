---
layout: post
title: Java Multithreading - Interrupting Threads
date: 2023-07-15 21:57 +0530
author: "Gaurav Kumar"
tags: "java multithreading theory"
categories: "multithreading"
---

This following post is based on the Udemy Course:
[Java Multithreading](https://www.udemy.com/course/java-multithreading/learn/lecture/107238#content)

---

## UNDERSTANDING INTERRUPT USING CODE

The main thread pauses for 500 milliseconds (0.5 seconds) using Thread.sleep(500). This allows the t1 thread to run for a short time before the main thread interrupts it. After the pause, the main thread interrupts the t1 thread by calling t1.interrupt(). This sends an interrupt signal to the t1 thread, which can be used to gracefully stop the execution or handle the interruption.  

The important thing to note in this example is that the t1 thread will still be able to complete what it's doing. In the next code example, we will look at how to detect if the thread has been interrupted and take any necessary action.

```java
package com.threads;

import java.util.Random;

public class App {

 public static void main(String[] args) throws InterruptedException {
  
  System.out.println("Started.");
  
  Thread t1 = new Thread(new Runnable() {
   
   Random random = new Random();
   
   @Override
   public void run() {
    
    //1E8 = 1x10^8
    //this code will take a while to complete
    for(int i=0; i<1E8; i++) {
     Math.sin(random.nextDouble());
    }
    
   }
  });
  
  t1.start();
  
  Thread.sleep(500);
  t1.interrupt();
  
  t1.join();
  
  System.out.println("Finished.");

 }

}
```

### USING INTERRUPTS

Within each iteration of the loop in Thread t1, there is a check using ```Thread.currentThread().isInterrupted()``` to see if the current thread (t1) has been interrupted. If it has, the program prints the name of the current thread followed by "has been interrupted" and breaks out of the loop.

```java
package com.threads;

import java.util.Random;

public class App {

 public static void main(String[] args) throws InterruptedException {
  
  System.out.println(Thread.currentThread().getName() + " has started");
  
  Thread t1 = new Thread(new Runnable() {
   
   Random random = new Random();
   
   @Override
   public void run() {
    
    
    for(int i=0; i<1E8; i++) {
     
     if(Thread.currentThread().isInterrupted()) {
      System.out.println(Thread.currentThread().getName() + " has been interrupted");
      break;
     }
     
     Math.sin(random.nextDouble());
    }

    System.out.println(Thread.currentThread().getName() + " has finished.");
    
   }
  });
  
  t1.start();
  
  Thread.sleep(500);
  t1.interrupt();
  
  t1.join();
  
  System.out.println(Thread.currentThread().getName() + " has finised");

 }

}
```

OUTPUT:

```text
main has started
Thread-0 has been interrupted.
Thread-0 has finished.
main has finised
```

### ADDING THREAD.SLEEP

In the below code, we have removed the check ```Thread.currentThread().isInterrupted()```. But, we have added ```Thread.sleep()``` which internally checks the status of the thread if it's interrupted. In that case, it will throw an InterruptedException.

```text
main has started
java.lang.InterruptedException: sleep interrupted
 at java.base/java.lang.Thread.sleep(Native Method)
 at thread101/com.threads.App$1.run(App.java:28)
 at java.base/java.lang.Thread.run(Thread.java:829)
```

```java
package com.threads;

import java.util.Random;

public class App {

 public static void main(String[] args) throws InterruptedException {
  
  System.out.println(Thread.currentThread().getName() + " has started");
  
  Thread t1 = new Thread(new Runnable() {
   
   Random random = new Random();
   
   @Override
   public void run() {
    
    
    for(int i=0; i<1E8; i++) {
     
     /*
      * if(Thread.currentThread().isInterrupted()) {
      * System.out.println(Thread.currentThread().getName() +
      * " has been interrupted."); break; }
      */
     
     try {
      Thread.sleep(100);
     } catch (InterruptedException e) {
      e.printStackTrace();
     }
     
     Math.sin(random.nextDouble());
    }
    
    System.out.println(Thread.currentThread().getName() + " has finished.");
    
   }
  });
  
  t1.start();
  
  Thread.sleep(500);
  t1.interrupt();
  
  t1.join();
  
  System.out.println(Thread.currentThread().getName() + " has finised");

 }

}
```
