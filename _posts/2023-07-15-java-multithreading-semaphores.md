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

## USAGE

The following program initializes a semaphore with one permit, which means only one thread can acquire the semaphore at a time. In the main method, the program prints "1" and then checks the number of available permits using semaphore.availablePermits(), which should be 1 initially. Next, the program calls semaphore.acquire(), which tries to acquire the semaphore. Since there is one permit available, the program successfully acquires it, prints "2", and checks the number of available permits again (which should be 0 now). After that, the program calls semaphore.acquire() again, but this time it will be blocked because there are no permits available. So, the program pauses at this point, and "3" is not printed.

```java
package com.threads;

import java.util.concurrent.Semaphore;

public class App {
 

 private static Semaphore semaphore = new Semaphore(1);
 
 public static void main(String[] args) {
  
  System.out.println("1");
  System.out.println("Available Permits: " + semaphore.availablePermits());
  
  try {
   semaphore.acquire();
  } catch (InterruptedException e) {
   e.printStackTrace();
  }
  
  System.out.println("2");
  System.out.println("Available Permits: " + semaphore.availablePermits());
  
  try {
   semaphore.acquire();
  } catch (InterruptedException e) {
   e.printStackTrace();
  }
  
  System.out.println("3");
  
 }
 
}
```

### THREAD DUMP

```text
4236:
2023-07-15 20:54:18
Full thread dump OpenJDK 64-Bit Server VM (11.0.15+10-LTS mixed mode):

Threads class SMR info:
_java_thread_list=0x00000283672076e0, length=10, elements={
0x000002833935a800, 0x0000028367140000, 0x000002836716d800, 0x00000283671c0800,
0x00000283671c1800, 0x00000283671c3000, 0x00000283671c8000, 0x00000283671cf000,
0x0000028367202000, 0x000002836736f000
}

"main" #1 prio=5 os_prio=0 cpu=31.25ms elapsed=16.97s tid=0x000002833935a800 nid=0xa9a4 waiting on condition  [0x000000c183bff000]
   java.lang.Thread.State: WAITING (parking)
 at jdk.internal.misc.Unsafe.park(java.base@11.0.15/Native Method)
 - parking to wait for  <0x0000000622260b98> (a java.util.concurrent.Semaphore$NonfairSync)
 at java.util.concurrent.locks.LockSupport.park(java.base@11.0.15/LockSupport.java:194)
 at java.util.concurrent.locks.AbstractQueuedSynchronizer.parkAndCheckInterrupt(java.base@11.0.15/AbstractQueuedSynchronizer.java:885)
 at java.util.concurrent.locks.AbstractQueuedSynchronizer.doAcquireSharedInterruptibly(java.base@11.0.15/AbstractQueuedSynchronizer.java:1039)
 at java.util.concurrent.locks.AbstractQueuedSynchronizer.acquireSharedInterruptibly(java.base@11.0.15/AbstractQueuedSynchronizer.java:1345)
 at java.util.concurrent.Semaphore.acquire(java.base@11.0.15/Semaphore.java:318)
 at com.threads.App.main(thread101/App.java:25)

"Reference Handler" #2 daemon prio=10 os_prio=2 cpu=0.00ms elapsed=16.94s tid=0x0000028367140000 nid=0xa48 waiting on condition  [0x000000c1842ff000]
   java.lang.Thread.State: RUNNABLE
 at java.lang.ref.Reference.waitForReferencePendingList(java.base@11.0.15/Native Method)
 at java.lang.ref.Reference.processPendingReferences(java.base@11.0.15/Reference.java:241)
 at java.lang.ref.Reference$ReferenceHandler.run(java.base@11.0.15/Reference.java:213)

"Finalizer" #3 daemon prio=8 os_prio=1 cpu=0.00ms elapsed=16.94s tid=0x000002836716d800 nid=0x7ea0 in Object.wait()  [0x000000c1843fe000]
   java.lang.Thread.State: WAITING (on object monitor)
 at java.lang.Object.wait(java.base@11.0.15/Native Method)
 - waiting on <0x0000000622408f98> (a java.lang.ref.ReferenceQueue$Lock)
 at java.lang.ref.ReferenceQueue.remove(java.base@11.0.15/ReferenceQueue.java:155)
 - waiting to re-lock in wait() <0x0000000622408f98> (a java.lang.ref.ReferenceQueue$Lock)
 at java.lang.ref.ReferenceQueue.remove(java.base@11.0.15/ReferenceQueue.java:176)
 at java.lang.ref.Finalizer$FinalizerThread.run(java.base@11.0.15/Finalizer.java:170)

"Signal Dispatcher" #4 daemon prio=9 os_prio=2 cpu=0.00ms elapsed=16.93s tid=0x00000283671c0800 nid=0x9088 runnable  [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE

"Attach Listener" #5 daemon prio=5 os_prio=2 cpu=0.00ms elapsed=16.93s tid=0x00000283671c1800 nid=0x89b8 waiting on condition  [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE

"Service Thread" #6 daemon prio=9 os_prio=0 cpu=0.00ms elapsed=16.92s tid=0x00000283671c3000 nid=0x8614 runnable  [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE

"C2 CompilerThread0" #7 daemon prio=9 os_prio=2 cpu=15.63ms elapsed=16.92s tid=0x00000283671c8000 nid=0x9974 waiting on condition  [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE
   No compile task

"C1 CompilerThread0" #10 daemon prio=9 os_prio=2 cpu=15.63ms elapsed=16.92s tid=0x00000283671cf000 nid=0x9cd8 waiting on condition  [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE
   No compile task

"Sweeper thread" #11 daemon prio=9 os_prio=2 cpu=0.00ms elapsed=16.92s tid=0x0000028367202000 nid=0x118c runnable  [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE

"Common-Cleaner" #12 daemon prio=8 os_prio=1 cpu=0.00ms elapsed=16.89s tid=0x000002836736f000 nid=0x9db0 in Object.wait()  [0x000000c184aff000]
   java.lang.Thread.State: TIMED_WAITING (on object monitor)
 at java.lang.Object.wait(java.base@11.0.15/Native Method)
 - waiting on <0x0000000622574190> (a java.lang.ref.ReferenceQueue$Lock)
 at java.lang.ref.ReferenceQueue.remove(java.base@11.0.15/ReferenceQueue.java:155)
 - waiting to re-lock in wait() <0x0000000622574190> (a java.lang.ref.ReferenceQueue$Lock)
 at jdk.internal.ref.CleanerImpl.run(java.base@11.0.15/CleanerImpl.java:148)
 at java.lang.Thread.run(java.base@11.0.15/Thread.java:829)
 at jdk.internal.misc.InnocuousThread.run(java.base@11.0.15/InnocuousThread.java:161)

"VM Thread" os_prio=2 cpu=0.00ms elapsed=16.95s tid=0x000002836713d000 nid=0x2de0 runnable  

"GC Thread#0" os_prio=2 cpu=0.00ms elapsed=16.97s tid=0x0000028339371800 nid=0x65a8 runnable  

"G1 Main Marker" os_prio=2 cpu=0.00ms elapsed=16.97s tid=0x00000283393f4800 nid=0x31bc runnable  

"G1 Conc#0" os_prio=2 cpu=0.00ms elapsed=16.97s tid=0x00000283393f7000 nid=0xa9a0 runnable  

"G1 Refine#0" os_prio=2 cpu=0.00ms elapsed=16.96s tid=0x000002836679c800 nid=0x5394 runnable  

"G1 Young RemSet Sampling" os_prio=2 cpu=0.00ms elapsed=16.96s tid=0x000002836679f800 nid=0x7b48 runnable  
"VM Periodic Task Thread" os_prio=2 cpu=0.00ms elapsed=16.84s tid=0x000002836757d800 nid=0x9f64 waiting on condition  

JNI global refs: 8, weak refs: 0
```

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

The given code represents a Connection class that manages connections to a limited resource (like a database). It uses a special object called a semaphore to control how many connections can be used at the same time. The code ensures that only a maximum of 10 connections can be taken at once. When a thread wants to use a connection, it needs to ask for permission from the semaphore. If there are available connections, the thread can take one and start doing its work. If all connections are already taken, the thread has to wait until a connection becomes available. Once a thread finishes using a connection, it releases it, allowing other threads to use it. The code also takes care of handling any exceptions that might occur during the connection process. Overall, the code ensures that the number of connections doesn't exceed the allowed limit and that different threads can use the connections fairly.

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
