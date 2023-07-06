---
layout: post
title: Java Multithreading - Thread Pools
date: 2023-07-06 21:19 +0530
author: "Gaurav Kumar"
tags: "java multithreading theory"
categories: "multithreading"
---

This following post is based on the Udemy Course:
[Java Multithreading](https://www.udemy.com/course/java-multithreading/learn/lecture/107238#content)

---

We will use the ExecutorService class to create a thread pool. Think of it as have multiple threads which will act as workers in a factory. We will submit/assign them certain tasks and they are supposed to complete them. If the thread pool size is x, then there will be x number of concurrently working workers on the assigned tasks.

> An ExecutorService can be shut down, which will cause it to reject new tasks. Two different methods are provided for shutting down an ExecutorService. The shutdown() method will allow previously submitted tasks to execute before terminating, while the shutdownNow() method prevents waiting tasks from starting and attempts to stop currently executing tasks. Upon termination, an executor has no tasks actively executing, no tasks awaiting execution, and no new tasks can be submitted. An unused ExecutorService should be shut down to allow reclamation of its resources.  
[ExecutorService](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ExecutorService.html)

```java
package demo;

import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.TimeUnit;

class Processor implements Runnable{
 
 private int id;
 
 public Processor(int id) {
  this.id = id;
 }
 
 @Override
 public void run() {
  
  System.out.println("Starting Thread: " + id);
  
  //simulate some work which can take some time
  try {
   Thread.sleep(2000); 
  }catch(InterruptedException e) {
   //e.printStackTrace();
  }
  
  System.out.println("Finished Thread: " + id);
  
 }
 
}

public class App {

 public static void main(String[] args) {
  
  //we will use an executor service to have multiple threads work on multiple tasks assigned to them
  //here, are setting the Thread Pool size to 2. So, there will be 2 workers or threads which will work on the tasks given.
  //when one of the tasks is complete, it will keep taking the other tasks until they are complete
  ExecutorService executorService = Executors.newFixedThreadPool(5);
  
  //submit 10 tasks
  for(int i=0; i<10; i++) {
   executorService.submit(new Processor(i));
  }
  
  //disable new tasks from being submitted
  executorService.shutdown();
  
  System.out.println("All tasks submitted.\n");
  
  try {
   
   int time = 2;
   
   //wait a while for existing tasks to terminate
   //executorService.awaitTermination(time, TimeUnit.SECONDS);
   
   //cancel currently executing tasks
   
   //awaitTermination blocks until all tasks have completed execution after a shutdownrequest, or the timeout occurs, or the current thread is interrupted, whichever happens first.
   //returns true if this executor terminated and false if the timeout elapsed before termination
   if (executorService.awaitTermination(time, TimeUnit.SECONDS))
    System.out.println("All tasks completed.");
   else {
    //attempts to stop all actively executing tasks, halts theprocessing of waiting tasks, and returns a list of the tasksthat were awaiting execution. 
    executorService.shutdownNow();
    System.out.println("Shutting down pending tasks since they did not complete within " + time + " " + TimeUnit.SECONDS);
   }
   
  } catch (InterruptedException e) {
   e.printStackTrace();
  }
  
 }
 
}
```

OUTPUT:  

```text
All tasks submitted.

Starting Thread: 3
Starting Thread: 4
Starting Thread: 0
Starting Thread: 1
Starting Thread: 2
Finished Thread: 2
Finished Thread: 1
Finished Thread: 0
Finished Thread: 4
Starting Thread: 5
Finished Thread: 3
Starting Thread: 7
Starting Thread: 6
Starting Thread: 8
Starting Thread: 9
Finished Thread: 6
Finished Thread: 5
Finished Thread: 9
Finished Thread: 7
Shutting down pending tasks since they did not complete within 2 SECONDS
Finished Thread: 8
```

OUTPUT when time=10:  

```text
All tasks submitted.

Starting Thread: 4
Starting Thread: 3
Starting Thread: 2
Starting Thread: 1
Starting Thread: 0
Finished Thread: 4
Finished Thread: 0
Finished Thread: 1
Finished Thread: 3
Finished Thread: 2
Starting Thread: 5
Starting Thread: 6
Starting Thread: 7
Starting Thread: 8
Starting Thread: 9
Finished Thread: 5
Finished Thread: 6
Finished Thread: 9
Finished Thread: 7
Finished Thread: 8
All tasks completed.
```
