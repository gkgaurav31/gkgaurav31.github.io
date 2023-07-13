---
layout: post
title: Java Multithreading - Deadlock
date: 2023-07-14 00:19 +0530
author: "Gaurav Kumar"
tags: "java multithreading theory"
categories: "multithreading"
---

This following post is based on the Udemy Course:
[Java Multithreading](https://www.udemy.com/course/java-multithreading/learn/lecture/107238#content)

---

## DEADLOCK

```text
   Process 1           Process 2
+-----------------+  +-----------------+
|    Resource A   |  |    Resource B   |
+-----------------+  +-----------------+
|   Request B     |  |   Request A     |
|    (waiting)    |  |    (waiting)    |
+-----------------+  +-----------------+
```

In the above image, there are two processes, Process 1 and Process 2, represented by the boxes. Each process requires access to two resources, Resource A and Resource B, represented by the labeled boxes.

Process 1 is currently holding Resource A but needs Resource B to continue its execution. However, Process 2 is holding Resource B and needs Resource A to proceed further. As a result, both processes are waiting for the resources that are held by the other process, creating a deadlock.

## EXAMPLE USING CODE

### Account.java

```java
package com.threads;

public class Account {

 private int balance = 10000;
 
 public void deposit(int amount) {
  balance += amount;
 }
 
 public void withdraw(int amount) {
  balance -= amount;
 }
 
 public int getBalance() {
  return balance;
 }
 
 //static method to transfer the given amount from account1 to account 2
 public static void transfer(Account account1, Account account2, int amount) {
  account1.withdraw(amount);
  account2.deposit(amount);
 }
 
}
```

### App.java

```java
package com.threads;

public class App {
 
 public static void main(String[] args) throws InterruptedException {
  
  Runner runner = new Runner();
  
  //thread1 -> will run firstThread() method is Runner
  Thread t1 = new Thread(new Runnable() {
   
   @Override
   public void run() {
    runner.firstThread();
   }
  });
  
  //thread2 -> will run secondThread() method is Runner
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

import java.util.Random;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class Runner {

 Account account1 = new Account();
 Account account2 = new Account();
 
 Lock lock1 = new ReentrantLock();
 Lock lock2 = new ReentrantLock();
 
 //gets both the locks and transfers the amount from account 1 to account 2
 public void firstThread() {
  
  lock1.lock();
  lock2.lock();
  
  for(int i=0; i<1000; i++) {
   Account.transfer(account1, account2, new Random().nextInt(100));
  }
  
  lock1.unlock();
  lock2.unlock();
  
 }
 
 //gets both the locks and transfers the amount from account 2 to account 1
 public void secondThread() {
  
  lock1.lock();
  lock2.lock();
  
  for(int i=0; i<1000; i++) {
   Account.transfer(account2, account1, new Random().nextInt(100));
  }
  
  lock1.unlock();
  lock2.unlock();
  
 }
 
 public void finished() {
  System.out.println("Total amount: " + (account1.getBalance() + account2.getBalance()));
 }
}
```

This will work as expected an show the output as - ```Total amount: 20000```

## DEADLOCK CODE

If we change the sequence in which the locks are obtained, it can lead to deadlock.

```java
 public void secondThread() {
  
  //changed the sequence of getting locks
  //now it's possible that thread1 got lock1 and thread2 got lock2. Then, thread1 is waiting for lock2 and thread2 is waiting for lock1. Due to this both the threads will be blocked.
  lock2.lock();
  lock1.lock();
  
  for(int i=0; i<1000; i++) {
   Account.transfer(account2, account1, new Random().nextInt(100));
  }
  
  lock1.unlock();
  lock2.unlock();
  
 }
```

## THREAD DUMP (DURING DEADLOCK)

> Command: jcmd PID Thread.print

In the thread dump, we can see that the deadlock was detected!

```text
20588:
2023-07-14 00:30:44
Full thread dump OpenJDK 64-Bit Server VM (11.0.15+10-LTS mixed mode):

Threads class SMR info:
_java_thread_list=0x00000271c16947b0, length=12, elements={
0x000002719151a000, 0x00000271c1255000, 0x00000271c1256800, 0x00000271c12df000,
0x00000271c12e0000, 0x00000271c12e2800, 0x00000271c12e5000, 0x00000271c1310800,
0x00000271c1321000, 0x00000271c148b800, 0x00000271c178a800, 0x00000271c178b000
}

"main" #1 prio=5 os_prio=0 cpu=46.88ms elapsed=29.55s tid=0x000002719151a000 nid=0x7fb8 in Object.wait()  [0x000000813dafe000]
   java.lang.Thread.State: WAITING (on object monitor)
 at java.lang.Object.wait(java.base@11.0.15/Native Method)
 - waiting on <0x00000006222668b0> (a java.lang.Thread)
 at java.lang.Thread.join(java.base@11.0.15/Thread.java:1300)
 - waiting to re-lock in wait() <0x00000006222668b0> (a java.lang.Thread)
 at java.lang.Thread.join(java.base@11.0.15/Thread.java:1375)
 at com.threads.App.main(thread101/App.java:28)

"Reference Handler" #2 daemon prio=10 os_prio=2 cpu=0.00ms elapsed=29.53s tid=0x00000271c1255000 nid=0x5938 waiting on condition  [0x000000813e1fe000]
   java.lang.Thread.State: RUNNABLE
 at java.lang.ref.Reference.waitForReferencePendingList(java.base@11.0.15/Native Method)
 at java.lang.ref.Reference.processPendingReferences(java.base@11.0.15/Reference.java:241)
 at java.lang.ref.Reference$ReferenceHandler.run(java.base@11.0.15/Reference.java:213)

"Finalizer" #3 daemon prio=8 os_prio=1 cpu=0.00ms elapsed=29.53s tid=0x00000271c1256800 nid=0x1428 in Object.wait()  [0x000000813e2fe000]
   java.lang.Thread.State: WAITING (on object monitor)
 at java.lang.Object.wait(java.base@11.0.15/Native Method)
 - waiting on <0x0000000622408f98> (a java.lang.ref.ReferenceQueue$Lock)
 at java.lang.ref.ReferenceQueue.remove(java.base@11.0.15/ReferenceQueue.java:155)
 - waiting to re-lock in wait() <0x0000000622408f98> (a java.lang.ref.ReferenceQueue$Lock)
 at java.lang.ref.ReferenceQueue.remove(java.base@11.0.15/ReferenceQueue.java:176)
 at java.lang.ref.Finalizer$FinalizerThread.run(java.base@11.0.15/Finalizer.java:170)

"Signal Dispatcher" #4 daemon prio=9 os_prio=2 cpu=0.00ms elapsed=29.52s tid=0x00000271c12df000 nid=0x4d4c runnable  [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE

"Attach Listener" #5 daemon prio=5 os_prio=2 cpu=0.00ms elapsed=29.52s tid=0x00000271c12e0000 nid=0x3e74 waiting on condition  [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE

"Service Thread" #6 daemon prio=9 os_prio=0 cpu=0.00ms elapsed=29.52s tid=0x00000271c12e2800 nid=0x6a0c runnable  [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE

"C2 CompilerThread0" #7 daemon prio=9 os_prio=2 cpu=0.00ms elapsed=29.52s tid=0x00000271c12e5000 nid=0x7760 waiting on condition  [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE
   No compile task

"C1 CompilerThread0" #10 daemon prio=9 os_prio=2 cpu=0.00ms elapsed=29.52s tid=0x00000271c1310800 nid=0x75ec waiting on condition  [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE
   No compile task

"Sweeper thread" #11 daemon prio=9 os_prio=2 cpu=0.00ms elapsed=29.52s tid=0x00000271c1321000 nid=0x4470 runnable  [0x0000000000000000]
   java.lang.Thread.State: RUNNABLE

"Common-Cleaner" #12 daemon prio=8 os_prio=1 cpu=0.00ms elapsed=29.48s tid=0x00000271c148b800 nid=0x7cb0 in Object.wait()  [0x000000813e9ff000]
   java.lang.Thread.State: TIMED_WAITING (on object monitor)
 at java.lang.Object.wait(java.base@11.0.15/Native Method)
 - waiting on <0x0000000622574190> (a java.lang.ref.ReferenceQueue$Lock)
 at java.lang.ref.ReferenceQueue.remove(java.base@11.0.15/ReferenceQueue.java:155)
 - waiting to re-lock in wait() <0x0000000622574190> (a java.lang.ref.ReferenceQueue$Lock)
 at jdk.internal.ref.CleanerImpl.run(java.base@11.0.15/CleanerImpl.java:148)
 at java.lang.Thread.run(java.base@11.0.15/Thread.java:829)
 at jdk.internal.misc.InnocuousThread.run(java.base@11.0.15/InnocuousThread.java:161)

"Thread-0" #13 prio=5 os_prio=0 cpu=0.00ms elapsed=29.42s tid=0x00000271c178a800 nid=0x7860 waiting on condition  [0x000000813ebfe000]
   java.lang.Thread.State: WAITING (parking)
 at jdk.internal.misc.Unsafe.park(java.base@11.0.15/Native Method)
 - parking to wait for  <0x0000000622266780> (a java.util.concurrent.locks.ReentrantLock$NonfairSync)
 at java.util.concurrent.locks.LockSupport.park(java.base@11.0.15/LockSupport.java:194)
 at java.util.concurrent.locks.AbstractQueuedSynchronizer.parkAndCheckInterrupt(java.base@11.0.15/AbstractQueuedSynchronizer.java:885)
 at java.util.concurrent.locks.AbstractQueuedSynchronizer.acquireQueued(java.base@11.0.15/AbstractQueuedSynchronizer.java:917)
 at java.util.concurrent.locks.AbstractQueuedSynchronizer.acquire(java.base@11.0.15/AbstractQueuedSynchronizer.java:1240)
 at java.util.concurrent.locks.ReentrantLock.lock(java.base@11.0.15/ReentrantLock.java:267)
 at com.threads.Runner.firstThread(thread101/Runner.java:19)
 at com.threads.App$1.run(thread101/App.java:13)
 at java.lang.Thread.run(java.base@11.0.15/Thread.java:829)

"Thread-1" #14 prio=5 os_prio=0 cpu=0.00ms elapsed=29.42s tid=0x00000271c178b000 nid=0x5c3c waiting on condition  [0x000000813ecfe000]
   java.lang.Thread.State: WAITING (parking)
 at jdk.internal.misc.Unsafe.park(java.base@11.0.15/Native Method)
 - parking to wait for  <0x0000000622266750> (a java.util.concurrent.locks.ReentrantLock$NonfairSync)
 at java.util.concurrent.locks.LockSupport.park(java.base@11.0.15/LockSupport.java:194)
 at java.util.concurrent.locks.AbstractQueuedSynchronizer.parkAndCheckInterrupt(java.base@11.0.15/AbstractQueuedSynchronizer.java:885)
 at java.util.concurrent.locks.AbstractQueuedSynchronizer.acquireQueued(java.base@11.0.15/AbstractQueuedSynchronizer.java:917)
 at java.util.concurrent.locks.AbstractQueuedSynchronizer.acquire(java.base@11.0.15/AbstractQueuedSynchronizer.java:1240)
 at java.util.concurrent.locks.ReentrantLock.lock(java.base@11.0.15/ReentrantLock.java:267)
 at com.threads.Runner.secondThread(thread101/Runner.java:33)
 at com.threads.App$2.run(thread101/App.java:21)
 at java.lang.Thread.run(java.base@11.0.15/Thread.java:829)

"VM Thread" os_prio=2 cpu=0.00ms elapsed=29.53s tid=0x00000271c1254800 nid=0x1f74 runnable  

"GC Thread#0" os_prio=2 cpu=0.00ms elapsed=29.55s tid=0x0000027191531000 nid=0x2bcc runnable  

"G1 Main Marker" os_prio=2 cpu=0.00ms elapsed=29.55s tid=0x00000271915b4800 nid=0x81a0 runnable  

"G1 Conc#0" os_prio=2 cpu=0.00ms elapsed=29.55s tid=0x00000271915b6800 nid=0x7b70 runnable  

"G1 Refine#0" os_prio=2 cpu=0.00ms elapsed=29.54s tid=0x00000271c08be000 nid=0x1020 runnable  

"G1 Young RemSet Sampling" os_prio=2 cpu=0.00ms elapsed=29.54s tid=0x00000271c08c1000 nid=0x3b64 runnable  
"VM Periodic Task Thread" os_prio=2 cpu=0.00ms elapsed=29.43s tid=0x00000271c153e800 nid=0x49a8 waiting on condition  

JNI global refs: 8, weak refs: 0


Found one Java-level deadlock:
=============================
"Thread-0":
  waiting for ownable synchronizer 0x0000000622266780, (a java.util.concurrent.locks.ReentrantLock$NonfairSync),
  which is held by "Thread-1"
"Thread-1":
  waiting for ownable synchronizer 0x0000000622266750, (a java.util.concurrent.locks.ReentrantLock$NonfairSync),
  which is held by "Thread-0"

Java stack information for the threads listed above:
===================================================
"Thread-0":
 at jdk.internal.misc.Unsafe.park(java.base@11.0.15/Native Method)
 - parking to wait for  <0x0000000622266780> (a java.util.concurrent.locks.ReentrantLock$NonfairSync)
 at java.util.concurrent.locks.LockSupport.park(java.base@11.0.15/LockSupport.java:194)
 at java.util.concurrent.locks.AbstractQueuedSynchronizer.parkAndCheckInterrupt(java.base@11.0.15/AbstractQueuedSynchronizer.java:885)
 at java.util.concurrent.locks.AbstractQueuedSynchronizer.acquireQueued(java.base@11.0.15/AbstractQueuedSynchronizer.java:917)
 at java.util.concurrent.locks.AbstractQueuedSynchronizer.acquire(java.base@11.0.15/AbstractQueuedSynchronizer.java:1240)
 at java.util.concurrent.locks.ReentrantLock.lock(java.base@11.0.15/ReentrantLock.java:267)
 at com.threads.Runner.firstThread(thread101/Runner.java:19)
 at com.threads.App$1.run(thread101/App.java:13)
 at java.lang.Thread.run(java.base@11.0.15/Thread.java:829)
"Thread-1":
 at jdk.internal.misc.Unsafe.park(java.base@11.0.15/Native Method)
 - parking to wait for  <0x0000000622266750> (a java.util.concurrent.locks.ReentrantLock$NonfairSync)
 at java.util.concurrent.locks.LockSupport.park(java.base@11.0.15/LockSupport.java:194)
 at java.util.concurrent.locks.AbstractQueuedSynchronizer.parkAndCheckInterrupt(java.base@11.0.15/AbstractQueuedSynchronizer.java:885)
 at java.util.concurrent.locks.AbstractQueuedSynchronizer.acquireQueued(java.base@11.0.15/AbstractQueuedSynchronizer.java:917)
 at java.util.concurrent.locks.AbstractQueuedSynchronizer.acquire(java.base@11.0.15/AbstractQueuedSynchronizer.java:1240)
 at java.util.concurrent.locks.ReentrantLock.lock(java.base@11.0.15/ReentrantLock.java:267)
 at com.threads.Runner.secondThread(thread101/Runner.java:33)
 at com.threads.App$2.run(thread101/App.java:21)
 at java.lang.Thread.run(java.base@11.0.15/Thread.java:829)

Found 1 deadlock.
```

## FIXING THE DEADLOCK

The idea is to ensure that either both the locks are acquired or none of them.

With the locks being released in the finally blocks, it helps prevent deadlocks from occurring. If one thread acquires lock1 and the other thread acquires lock2, and they both reach the finally block, each thread will release the lock it acquired. This ensures that even if one thread holds one lock and is waiting for the other, both locks will eventually be released, allowing progress to be made.

```java
package com.threads;

import java.util.Random;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class Runner {

 Account account1 = new Account();
 Account account2 = new Account();

 Lock lock1 = new ReentrantLock();
 Lock lock2 = new ReentrantLock();
 
 public void acquireLocks(Lock lock1, Lock lock2) throws InterruptedException {
  
  //both the threads will keep running this loop while trying to acquire both the locks
  //if one of the threads get lock1, and other gets the other lock2, then the finally block will be executed for those threads
  //in finally, they will "release" the locks that they had acquired, thereby avoiding any chance of deadlock
  while (true) {

   boolean isFirstLocked = false;
   boolean isSecondLocked = false;

   try {

    //Acquires the lock if it is available and returns immediately with the value true.
    //If the lock is not available then this method will return immediately with the value false. 
    isFirstLocked = lock1.tryLock();
    isSecondLocked = lock2.tryLock();

   } finally {

    if (isFirstLocked && isSecondLocked) {
     return;
    }

    if (isFirstLocked)
     lock1.unlock();

    if (isSecondLocked)
     lock2.unlock();

   }

   // try again for some time
   Thread.sleep(1);

  }

 }

 public void firstThread() throws InterruptedException {

  for (int i = 0; i < 1000; i++) {
   
   //thread1 will keep trying to acquire both the locks
   //this will only move forward after it gets both the locks
   acquireLocks(lock2, lock1);

   try {
    Account.transfer(account1, account2, new Random().nextInt(100));
   } finally {
    lock1.unlock();
    lock2.unlock();

   }

  }

 }

 public void secondThread() throws InterruptedException {

  for (int i = 0; i < 1000; i++) {
   
   //thread2 will keep trying to acquire both the locks
   //this will only move forward after it gets both the locks
   acquireLocks(lock2, lock1);

   try {
    Account.transfer(account2, account1, new Random().nextInt(100));
   } finally {
    lock1.unlock();
    lock2.unlock();
   }

  }

 }

 public void finished() {
  System.out.println("Total amount: " + (account1.getBalance() + account2.getBalance()));
 }
}
```
