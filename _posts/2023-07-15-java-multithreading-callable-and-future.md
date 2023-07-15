---
layout: post
title: Java Multithreading - Callable and Future
date: 2023-07-15 21:32 +0530
author: "Gaurav Kumar"
tags: "java multithreading theory"
categories: "multithreading"
---

This following post is based on the Udemy Course:
[Java Multithreading](https://www.udemy.com/course/java-multithreading/learn/lecture/107238#content)

---

## CALLABLE AND FUTURE

Callable is an interface that represents a task or computation that can be executed in a separate thread. It is similar to the Runnable interface, but with a key difference: a Callable can return a result and throw checked exceptions.  

Future is an interface that represents the result of a computation performed by a Callable. When you submit a Callable to an executor service (a thread pool), it returns a Future object. This Future object allows you to track the progress of the computation and retrieve the result once it's available.

The Callable interface includes a parameter ```<V>``` which is the result type of method.  
[Callable](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/concurrent/Callable.html)

```java
@FunctionalInterface
public interface Callable<V> {
    /**
     * Computes a result, or throws an exception if unable to do so.
     *
     * @return computed result
     * @throws Exception if unable to compute a result
     */
    V call() throws Exception;
}
```

The Future class has some useful methods like ```cancel()``` and ```isDone()```.  

cancel() is a method that allows you to cancel the execution of a Future task. When you call cancel(), it attempts to cancel the associated computation. If the computation has not started yet, it will be canceled successfully, and subsequent calls to isDone() will return true. However, if the computation has already started or completed, the cancellation might not be possible or effective.  

isDone() is a method that checks whether the associated computation of a Future task has completed. It returns true if the computation is done or has been canceled, and false if the computation is still in progress. This method is useful to determine if the result of the computation is available or if it's still being calculated.

## USING CALLCABLE AND FUTURE IN CODE

```java
package com.threads;

import java.util.Random;
import java.util.concurrent.Callable;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

public class App {
 
 public static void main(String[] args) {
  
  ExecutorService executorService = Executors.newCachedThreadPool();
  
  //The executor service returns a Future object representing the computation.
  Future<Integer> future =  executorService.submit(new Callable<Integer>() {

   @Override
   public Integer call() throws Exception {
    
    Random random = new Random();
    int duration = random.nextInt(2000);
    
    Thread.sleep(duration);
    
    return duration;
   }
  });
  
  executorService.shutdown();
  
  try {
   //waits if necessary for the computation to complete, and then retrieves its result.
   //So, we don't need to really call executorService.await() 
   System.out.println("Sleeping for: " + future.get());
  } catch (InterruptedException e) {
   e.printStackTrace();
  } catch (ExecutionException e) {
   e.printStackTrace();
  }
  
 }
 
}
```

## EXCEPTION WITH CALLABLE

In the following code, if the duration is more than 1000 ms, it will throw an Exception.

### STACKTRACE

```text
java.util.concurrent.ExecutionException: java.io.IOException: waiting for too long.
 at java.base/java.util.concurrent.FutureTask.report(FutureTask.java:122)
 at java.base/java.util.concurrent.FutureTask.get(FutureTask.java:191)
 at thread101/com.threads.App.main(App.java:41)
Caused by: java.io.IOException: waiting for too long.
 at thread101/com.threads.App$1.call(App.java:27)
 at thread101/com.threads.App$1.call(App.java:1)
 at java.base/java.util.concurrent.FutureTask.run(FutureTask.java:264)
 at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1128)
 at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:628)
 at java.base/java.lang.Thread.run(Thread.java:829)
```

```java
package com.threads;

import java.io.IOException;
import java.util.Random;
import java.util.concurrent.Callable;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

public class App {
 
 public static void main(String[] args) {
  
  ExecutorService executorService = Executors.newCachedThreadPool();
  
  //The executor service returns a Future object representing the computation.
  Future<Integer> future =  executorService.submit(new Callable<Integer>() {

   @Override
   public Integer call() throws Exception {
    
    Random random = new Random();
    int duration = random.nextInt(2000);
    
    if(duration > 1000) {
     throw new IOException("waiting for too long."); //just for example
    }
    
    Thread.sleep(duration);
    
    return duration;
   }
  });
  
  executorService.shutdown();
  
  try {
   //waits if necessary for the computation to complete, and then retrieves its result.
   //So, we don't need to really call executorService.await() 
   System.out.println("Sleeping for: " + future.get());
  } catch (InterruptedException e) {
   e.printStackTrace();
  } catch (ExecutionException e) {
   e.printStackTrace();
  }
  
 }
 
}
```

## USING CALLCABLE WITHOUT RETURNING ANY RESULT

We will need to use the type as "?" and return Void.

```java
Future<?> future =  executorService.submit(new Callable<Void>() {

   @Override
   public Void call() {
    return null;
   }
  });
```

```java
package com.threads;

import java.util.concurrent.Callable;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

public class App {
 
 public static void main(String[] args) {
  
  ExecutorService executorService = Executors.newCachedThreadPool();
  
  Future<?> future =  executorService.submit(new Callable<Void>() {

   @Override
   public Void call(){
    
    //do some work
    
    return null;
   }
  });
  
  executorService.shutdown();
  
  try {
 System.out.println("Result is: " + future.get());
} catch (InterruptedException e) {
 e.printStackTrace();
} catch (ExecutionException e) {
 e.printStackTrace();
}
  
 }
 
}
```

OUTPUT:

```text
Result is: null
```
