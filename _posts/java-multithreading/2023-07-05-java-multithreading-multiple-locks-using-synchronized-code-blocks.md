---
layout: post
title: Java Multithreading - Multiple Locks; Using Synchronized Code Blocks
date: 2023-07-05 20:56 +0530
author: "Gaurav Kumar"
tags: "java multithreading theory"
categories: "multithreading"
---

This following post is based on the Udemy Course:
[Java Multithreading](https://www.udemy.com/course/java-multithreading/learn/lecture/107238#content)

---

### Basic App without threads

```java
package demo;

public class App {

 public static void main(String[] args) {
  
  //create a new Worker object and call its main method
  new Worker().main();
  
 }

}
```

```java
package demo;

import java.util.ArrayList;
import java.util.List;
import java.util.Random;

public class Worker {
 
 private Random random = new Random();
 
 private List<Integer> list1 = new ArrayList<>();
 private List<Integer> list2 = new ArrayList<>();

 //sleep for 1 second and add a random number to list1
 public void taskOne() {
  
  try {
   Thread.sleep(1);
  } catch (InterruptedException e) {
   e.printStackTrace();
  }
  
  list1.add(random.nextInt());
  
 }

 //sleep for 1 second and add a random number to list2
 public void taskTwo() {
  
  try {
   Thread.sleep(1);
  } catch (InterruptedException e) {
   e.printStackTrace();
  }
  
  list2.add(random.nextInt());
  
 }

 //run taskOne() and taskTwo() methods 1000 times each
 public void process() {
  
  for(int i=0; i<1000; i++) {
   taskOne();
   taskTwo();
  }

 }
    
 //call the process() method and print the time taken to process it. Also, print the size of list1 and list2
 public void main() {
  
  System.out.println("Starting...");
  long start = System.currentTimeMillis();
  
  process();
  
  long end = System.currentTimeMillis();
  
  System.out.println("Time Taken (in ms): " + (end-start));
  System.out.println("List1 Size: " + list1.size());
  System.out.println("List2 Size: " + list2.size());
  
  
 }
 
}
```

During my tests, the time taken was around 3 seconds for most runs.

```text
Starting...
Time Taken (in ms): 3111
List1 Size: 1000
List2 Size: 1000
```

### Process using Two Threads

Update the main() method in Worker class as below:

```java
 public void main() {
  
  System.out.println("Starting...");
  
  //record start time
  long start = System.currentTimeMillis();
  
  //create a new thread which will call the process() method
  Thread t1 = new Thread(new Runnable() {
   
   @Override
   public void run() {
    process();
   }
  });
  
  //another thread to do the same thing
  Thread t2 = new Thread(new Runnable() {
   
   @Override
   public void run() {
    process();
   }
  });
  
  //start both the threads
  t1.start();
  t2.start();
  
  //wait for both the threads to complete
  try {
   t1.join();
   t2.join();
  } catch (InterruptedException e) {
   e.printStackTrace();
  }
  
  //record the end time
  long end = System.currentTimeMillis();
  
  //print
  System.out.println("Time Taken (in ms): " + (end-start));
  System.out.println("List1 Size: " + list1.size());
  System.out.println("List2 Size: " + list2.size());
  
 }
```

Sample Output:  

```text
Starting...
Time Taken (in ms): 3104
List1 Size: 1994
List2 Size: 1996
```

Notice here that the size of list1 and list2 are unexpected. This is similar to the problem which we had seen earlier while incrementing the count variable using two different threads concurrently. This is not a good idea.

### Non-optimal solution

The problem with the previous code is that multiple threads can modify the same object concurrently by executing the same code block at the same time. This can lead to unexpected results. One way to fix this is by marking the method in which we are updating the lists as synchronized.

This would ensure that multiple threads don't try to update list1/list2 at the same time. But, this can cause the processing time to get doubled. This is because, before a thread can call the synchroized method, it will need to get the intrinsic lock of that object. So the other thread(s) will have to wait for the previous thread to complete. In the current example, taskOne and taskTwo methods are independent of each other as they modify different lists. Therefore, two different threads should be "allowed" to ideally call taskOne() and taskTwo() methods concurrently.

```text
Starting...
Time Taken (in ms): 6125
List1 Size: 2000
List2 Size: 2000
```

### Optimal solution using multiple locks

We'll create two "dummy" objects that will act as locks. Instead of using the synchronized keyword on the taskOne() and taskTwo() methods, we'll use a synchronized block of code within those methods. In this synchronized block, we specify the object whose intrinsic lock we want to use.  

In the example below, multiple threads can call the taskOne() or taskTwo() methods at the same time. However, they cannot access the synchronized block of code concurrently within any of these methods. This is useful because now the two threads can call taskOne and taskTwo at the same time (since it doesn't require the intrinsic lock of the Worker object).  

```java
 private Random random = new Random();
 
 private Object lock1 = new Object();
 private Object lock2 = new Object();
 
 private List<Integer> list1 = new ArrayList<>();
 private List<Integer> list2 = new ArrayList<>();

 public void taskOne() {
  
  synchronized (lock1) {
   try {
    Thread.sleep(1);
   } catch (InterruptedException e) {
    e.printStackTrace();
   }
   
   list1.add(random.nextInt());
  }
  
 }

 public void taskTwo() {
  
  synchronized (lock2) {
   try {
    Thread.sleep(1);
   } catch (InterruptedException e) {
    e.printStackTrace();
   }
   
   list2.add(random.nextInt());
  }
  
 }
```

The time taken is back to the expected value:  

```text
Starting...
Time Taken (in ms): 3073
List1 Size: 2000
List2 Size: 2000
```
