---
layout: post
title: Java Multithreading - Starting Java Threads
date: 2023-07-02 23:51 +0530
author: "Gaurav Kumar"
tags: "java multithreading theory"
categories: "multithreading"
---

This following post is based on the Udemy Course:
[Java Multithreading](https://www.udemy.com/course/java-multithreading/learn/lecture/107238#content)

## Java Multi-Threading: Starting Java Threads

We can create threads either by extending the Thread class or by implementing the Runnable interface. Here is an example of how it would look like.

### Extending Thread Class

```java
package com.learn;


class Runner extends Thread{

 public void run() {
  
  for(int i=0; i<10; i++) {
   
   System.out.println("Hello " + i);
   
   try {
    Thread.sleep(100);
   } catch (InterruptedException e) {
    e.printStackTrace();
   }
  }
 }
 
}

public class App {

 public static void main(String[] args) {
  
  Runner runner1 = new Runner();
  runner1.start(); //Don't call run method directly, otherwise it would just run in the main thread of the app
  
  Runner runner2 = new Runner();
  runner2.start();
  
 }
 
}
```

### Implement Runnable Interface

```java
package com.demo;

class Runner implements Runnable{

 @Override
 public void run() {

  for(int i=0; i<10; i++) {

   System.out.println("Hi " + i);

   try {
    Thread.sleep(100);
   } catch (InterruptedException e) {
    e.printStackTrace();
   }

  }

 }

}

public class MyApp {

 public static void main(String[] args) {

  Thread t1 = new Thread(new Runner());
  Thread t2 = new Thread(new Runner());
  
  t1.start();
  t2.start();
  
 }

}
```

### Using Anonymous Class

```java
package demo3;

public class App {

 public static void main(String[] args) {
  
  Thread t1 = new Thread(new Runnable() {
   
   @Override
   public void run() {
    
    for(int i=0; i<10; i++) {

     System.out.println("Hey " + i);

     try {
      Thread.sleep(100);
     } catch (InterruptedException e) {
      e.printStackTrace();
     }

    }
    
   }
  });
  
  t1.start();
  
 }

}
```
