---
layout: post
title: Java Multithreading - Semaphores
date: 2023-07-15 19:36 +0530
author: "Gaurav Kumar"
tags: "java multithreading theory"
categories: "multithreading"
---

This following post is based on the Udemy Course:
[Java Multithreading](https://www.udemy.com/course/java-multithreading/learn/lecture/107238#content)

---

## SEMAPHORES

> A counting semaphore. Conceptually, a semaphore maintains a set of permits. Each acquire() blocks if necessary until a permit is available, and then takes it. Each release() adds a permit, potentially releasing a blocking acquirer. However, no actual permit objects are used; the Semaphore just keeps a count of the number available and acts accordingly.
> Semaphores are often used to restrict the number of threads than can access some (physical or logical) resource.

[Oracle Doc](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/concurrent/Semaphore.html)  

Suppose you have a database server that can handle a maximum of 10 concurrent connections due to resource limitations. Each connection corresponds to a thread that needs to acquire a permit from the semaphore before using the database.  

In this case, you would initialize a semaphore with a value of 10, indicating the maximum number of connections available. Each thread (database client) would need to acquire a permit from the semaphore using the acquire() method before it can establish a connection and start executing database operations.  

When a thread calls the acquire() method, it tries to acquire a permit from the semaphore. If a permit is available (a database connection is available), the thread takes the permit and proceeds to establish a connection. If all permits are currently acquired (all connections are in use), the thread will be blocked and put into a waiting state until a permit becomes available (a connection is released).  

Once a thread finishes using the database connection, it releases the permit back to the semaphore using the release() method. This indicates that the connection is no longer in use and becomes available for other threads to acquire.  

By using the acquire() and release() methods with the semaphore, you ensure that the number of concurrent database connections does not exceed the server's capacity. Threads acquire a permit (connection) before accessing the database and release it when they are done, allowing other threads to acquire the permit and use the connection.  

This approach effectively manages access to the limited number of available database connections, preventing overload and ensuring fair utilization of the server's resources.  

## WITHOUT SEMAPHORE

### App.java

We will make use of a Cached Thread Pool in this example. It creates a thread pool that creates new threads as needed, but will reuse previously constructed threads when they are available. These pools will typically improve the performance of programs that execute many short-lived asynchronous tasks. Calls to execute will reuse previously constructed threads if available. If no existing thread is available, a new thread will be created and added to the pool. Threads that have not been used for sixty seconds are terminated and removed from the cache. Thus, a pool that remains idle for long enough will not consume any resources.

```java
package com.threads;

import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Semaphore;
import java.util.concurrent.TimeUnit;

public class App {
 
 static Semaphore semaphore = new Semaphore(10);

 public static void main(String[] args) {
  
  ExecutorService executorService = Executors.newCachedThreadPool();
  
  //submit 200 tasks
  for(int i=0; i<200; i++) {
   executorService.submit(new Runnable() {
    
    @Override
    public void run() {
     Connection.getInstance().connect();
    }
   });
  }
  
  executorService.shutdown();
  
  try {
   executorService.awaitTermination(1, TimeUnit.DAYS);
  } catch (InterruptedException e) {
   e.printStackTrace();
  }
  
 }
 
}
```

### Connection.java

Connection class is singleton here. In the following code, the number of connections can reach up to 200. In the next example, we will use Semaphore to limit the number of connections.

```java
package com.threads;

//Singleton
public class Connection {

 private static Connection connection = new Connection();
 private int numberOfConnections = 0;
 
 //private constructor so that other classes cannot create an object of Connection
 private Connection() {
  
 }
 
 public void connect() {
  
  synchronized (this) {
   numberOfConnections++;
   System.out.println("Current number of connections: " + numberOfConnections);
  }
  
  //do some work
  try {
   Thread.sleep(2000);
  } catch (InterruptedException e) {
   e.printStackTrace();
  }
  
  synchronized (this) {
   numberOfConnections--;
  }
  
 }
 
 public static Connection getInstance() {
  return connection;
 }
 
}
```

## USING SEMAPHORE TO LIMIT THE CONNECTIONS

```java
package com.threads;

import java.util.Random;
import java.util.concurrent.Semaphore;

//Singleton
public class Connection {

 private static Connection connection = new Connection();
 private int numberOfConnections = 0;
 
 /*
    limit connections to 10
    true means whichever thread gets first in the waiting pool (queue)
    waiting to acquire a resource, is first to obtain the permit.

    Note that I called it a pool!
    The reason is when you say "Queue", you're saying that things are
    scheduled to be FIFO (First In First Out) .. which is not always the case
    here!
    When you initialize the semaphore with Fairness, by setting its second
    argument to true, it will treat the waiting threads like FIFO.
    But,
    it doesn't have to be that way if you don't set on the fairness. the JVM
    may schedule the waiting threads in some other manner that it sees best
    (See the Java specifications for that).
 */
 
 private Semaphore semaphore = new Semaphore(10, true);
 
 //private constructor so that other classes cannot create an object of Connection
 private Connection() {
  
 }
 
 public void connect() {
  
  try {
   semaphore.acquire();
  } catch (InterruptedException e) {
   e.printStackTrace();
  }
  
  try {
   doConnect(); //By using try with finally, we are making sure that release() is called even when there is any exception while calling doConnect()
  }finally {
   semaphore.release();
  }
  
 }
 
 public void doConnect() {
  
  synchronized (this) { //atomic
   numberOfConnections++;
   System.out.println("Current number of connections: " + numberOfConnections);
  }
  
  //do some work
  try {
   System.out.println("Working on connections " + Thread.currentThread().getName());
   Thread.sleep(new Random().nextInt(2000));
  } catch (InterruptedException e) {
   e.printStackTrace();
  }
  
  synchronized (this) { //atomic
   numberOfConnections--;
   System.out.println("I'm done " + Thread.currentThread().getName() + " Connection is released , connection count: " + numberOfConnections);
  }
  
 }
 
 public static Connection getInstance() {
  return connection;
 }
 
}
```
