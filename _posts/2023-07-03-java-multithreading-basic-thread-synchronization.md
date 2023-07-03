---
layout: post
title: Java Multithreading - Basic Thread Synchronization
date: 2023-07-03 20:39 +0530
author: "Gaurav Kumar"
tags: "java multithreading theory"
categories: "multithreading"
---

This following post is based on the Udemy Course:
[Java Multithreading](https://www.udemy.com/course/java-multithreading/learn/lecture/107238#content)

---

When data is shared between multiple threads, a common issue arises with data caching. In the following example, there is a thread called "Processor" that contains a boolean variable named "running," initially set to true. The main program creates a new Processor thread that continues running as long as the value of "running" remains true. Meanwhile, the main thread waits for user input. Once the user presses enter, the shutdown() method is invoked on the previously created thread.  

Typically, this isn't a major problem. However, certain JVM implementations can cache the value of the "running" variable. Consequently, even if the main thread modifies the value while it's running, the Processor thread remains unaffected and continues to execute based on the cached value.  

To address this issue, the solution is to declare the "running" variable as volatile. By doing so, we ensure that it is not cached and always reflects the latest value.  

> Quoting John (Author): The same problems apply to static as far as I know. The problem here isn't with multiple copies of the variable as such; there's only one because there's only one object. So an instance variable should be fine. But each thread may cache the variable value internally, if it sees that it's not modified within the thread. Then this can go wrong if multiple thread modify the same variable, because the caching mechanism is only guaranteed to work for individual threads. This is why volatile is needed, to prevent cacheing. It's a bit like if you have access to someone's address book (in real life) and you memorise their phone number from it, because you know that they don't change their address book, and they only have one address book. You've "cached" it in your memory for efficiency. But if someone else has access to their address book, and changes it with an updated phone number for them, your plan comes undone.

```java
package demo;

import java.util.Scanner;

class Processor extends Thread{
 
 //In certain implementation of the JVM, the running variable can be cached because in Processor Thread, it does not expect it to be changed from another thread
 //In this demo, the value is being changed from the "main" thread.
 //To make sure that it runs for sure, mark this variable as volatile
 private boolean running = true; //-> change it to private volatile boolean running = true;
 
 @Override
 public void run() {
  
  while(running) {
   System.out.println("Hello");
  }
  
 }
 
 public void shutdown() {
  running = false;
 }
 
}

public class App {

 public static void main(String[] args) {
  
  //create a new thread
  Processor processor = new Processor();
  
  //start the thread
  processor.start();
  
  //wait for input from the user
  System.out.println("Press enter to stop.");
  Scanner scanner = new Scanner(System.in);
  scanner.nextLine();
  
  //call shutdown method to change the variable running to false
  processor.shutdown();
  
 }

}
```
