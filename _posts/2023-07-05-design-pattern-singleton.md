---
layout: post
title: "Design Pattern: Singleton"
date: 2023-07-05 23:17 +0530
author: "Gaurav Kumar"
tags: "java design_patterns"
categories: "design_patterns"
---

## SINGLETON

The objective is to ensure that a single object of a particular class can exist for an app.

**Thought Process:**

- We will have to make the constructor of that class private so that other classes cannot create objects using it.
- We will need to use a method in that class itself to return an object of that class, using the private constructor.
- That method should be allowed to be called when there are no objects of that class as well. So, we will need to mark it static.
- The method needs to create a new object when called for the first time and somehow store it. We can use an attribute in that class for this. The attribute will also need to be static because it needs to be accessed from a static method.

### APPROACH 1

In the following example, the logger attribute will be initialized when the Logger class is loaded by the JVM. This example would work well, except in the case which requires certain customizations during runtime. For example, let's say that the logger object needs to be created using certain arguments/parameters which will be available during runtime only. We cannot use this option in that scenario.

```java
package demo;

public class Logger {

 private static Logger logger = new Logger();

 private Logger() {

 }

 public static Logger getInstance() {
  return logger;
 }

}
```

To work-around the previous problem, we can create a method which will check if the logger object has been initialized already. If it is null, create an object of Logger. Otherwise, return it.

This code, however, is not thread-safe. Suppose, two thread t1 and t2 access the getInstance() method concurrently. Then they concurrently check if logger is null. For both threads, it is possible that the condition is true. In this case, t1 and t2 both will create objects for Logger which is undesirable.

### NON-THREAD SAFE

```java
package demo;

public class Logger {

 private static Logger logger;

 private Logger() {

 }

 public static Logger getInstance() {

  if(logger == null) {
   logger = new Logger();
  }

  return logger;

 }

}

```

### USING SYNCHRONIZED (NOT OPTIMAL)

The obvious solution for the previous problem is to mark the method getInstance() as synchronized. This will ensure that only one thread can access this method at a time.

The important thing to realize is that the issue could have happened only when the Logger object is created for the first time. After that, the existing object can just be returned without really requiring a lock. The synchronized method will also affected the performance since multiple threads cannot access getInstance() method even after the logger object has been created.

```java
package demo;

public class Logger {

 private static Logger logger;

 private Logger() {

 }

 public static synchronized Logger getInstance() {

  if(logger == null) {
   logger = new Logger();
  }

  return logger;

 }

}
```

### DOUBLE-CHECKED LOCKING (THREAD-SAFE AND OPTIMAL)

To make the code optimal, we can use the double-checked lock technique. The getInstance() method will be allowed to be concurrently called by multiple threads. However, if the logger object is null, one of the threads will need to take the lock on Logger class before creating the required object. We will need to add an if/else check within the synchronized code block so that the other threads do not try to re-create the object if it was created by the first thread which had acquired the lock initially.

```java
package demo;

public class Logger {

 private static Logger logger;

 private Logger() {

 }

 public static Logger getInstance() {

  if(logger == null) {

   synchronized (Logger.class) {

    if(logger == null) {
     logger = new Logger();
    }

   }

  }

  return logger;

 }

}
```
