---
layout: post
title: 'Design Pattern: Prototype'
date: 2023-07-10 21:24 +0530
author: "Gaurav Kumar"
tags: "java design_patterns"
categories: "design_patterns"
---

## PROTOTYPE

## IDEA

Imagine a situation where creating an object is a costly task. This can happen when we need certain information from a database or external service before we can create the object. If we have to create this object multiple times in our application, it can slow down the overall performance. In such cases, we can use "pre-created" prototype objects. The idea is to create some objects in advance with attributes that usually don't change. Then, whenever we need a new object, we can quickly make a copy of this pre-created object and adjust any specific attributes we need. This way, we can save time and improve the performance of our application.

## CODE WALKTHROUGH

### INTERFACE FOR PROTOTYPE

```java
package com.designpattern;

public interface NotebookPrototype<T> {
 T copy();
}
```

### CONCRETE CLASS IMPLEMENTING THE PROTOTYPE

```java
package com.designpattern;

public class Notebook implements NotebookPrototype<Notebook>{
 
 //common attributes
 private String bookTitle;
 private double bookPrice;
 
 //specialized attributes
 private String bookFact;
 
 @Override
 //This method does the main work
 //We get a prototype pre-created object from the registry
 //Then we can call the copy method on that prototype object to return a new Notebook with identical attributes
 //Once we have that, we can update the specialized attributes as we need
 public Notebook copy() {
  
  Notebook copy = new Notebook();
  
  copy.bookTitle = this.bookTitle;
  copy.bookPrice = this.bookPrice;
  
  //bookFact will be different for each book, so we don't copy that

  return copy;
  
 }

 public String getBookTitle() {
  return bookTitle;
 }

 public void setBookTitle(String bookTitle) {
  this.bookTitle = bookTitle;
 }

 public double getBookPrice() {
  return bookPrice;
 }

 public void setBookPrice(double bookPrice) {
  this.bookPrice = bookPrice;
 }

 public String getBookFact() {
  return bookFact;
 }

 public void setBookFact(String bookFact) {
  this.bookFact = bookFact;
 }

 @Override
 public String toString() {
  return "Notebook [bookTitle=" + bookTitle + ", bookPrice=" + bookPrice + ", bookFact=" + bookFact + "]";
 }
 
}
```

### REGISTRY CLASS TO STORE AND RETRIEVE PROTOTYPE OBJECTS

```java
package com.designpattern;

import java.util.HashMap;
import java.util.Map;

public class NotebookRegistry {

 private Map<String, Notebook> map;
 
 public NotebookRegistry() {
  map = new HashMap<>();
 }
 
 public void register(Notebook notebook, String key) {
  map.put(key, notebook);
 }
 
 public Notebook get(String key) {
  return map.get(key);
 }
 
}
```

### HELPER CLASS TO GENERATE RANDOM FACTS FOR SPECIALIZED ATTRIBUTES (OPTIONAL)

```java
package com.designpattern;

import java.util.Random;

public class RandomFactGenerator {
 
    private static final String[] facts = {
        "The Eiffel Tower in Paris was originally intended to be a temporary structure.",
        "The world's oldest known living tree is over 4,800 years old.",
        "Honey never spoils. Archaeologists have found pots of honey in ancient Egyptian tombs that are over 3,000 years old and still perfectly edible.",
        "The average person walks the equivalent of three times around the world in a lifetime.",
        "The shortest war in history was between Britain and Zanzibar in 1896. It lasted only 38 minutes.",
        "Octopuses have three hearts.",
        "The Great Wall of China is visible from space.",
        "A group of flamingos is called a flamboyance.",
        "The Mona Lisa has no eyebrows.",
        "Giraffes have the same number of neck vertebrae as humans."
    };

    public static String getRandomFact() {
        Random random = new Random();
        int index = random.nextInt(facts.length);
        return facts[index];
    }

    public static void main(String[] args) {
        String randomFact = getRandomFact();
        System.out.println("Random Fact: " + randomFact);
    }
}
```

### CLIENT

```java
package com.demo;

import com.designpattern.Notebook;
import com.designpattern.NotebookRegistry;
import com.designpattern.RandomFactGenerator;

public class Client {
 
 public static void main(String[] args) {
  
  NotebookRegistry notebookRegistry = new NotebookRegistry();
  fillRegistry(notebookRegistry);
  
  //IMPORTANT NOTE: make sure to call the copy() method after getting the prototype objects from the registry
  Notebook divergentCopy1 = notebookRegistry.get(Books.DIVERGENT.toString()).copy();
  divergentCopy1.setBookFact(RandomFactGenerator.getRandomFact());
  
  Notebook divergentCopy2 = notebookRegistry.get(Books.DIVERGENT.toString()).copy();
  divergentCopy2.setBookFact(RandomFactGenerator.getRandomFact());
  
  Notebook divergentCopy3 = notebookRegistry.get(Books.DIVERGENT.toString()).copy();
  divergentCopy3.setBookFact(RandomFactGenerator.getRandomFact());
  
  Notebook inceptionCopy1 = notebookRegistry.get(Books.INCEPTION.toString()).copy();
  inceptionCopy1.setBookFact(RandomFactGenerator.getRandomFact());
  
  //we have added a toString() method in Notebook class to print the attributes when calling sysout
  System.out.println(divergentCopy1);
  System.out.println(divergentCopy2);
  System.out.println(divergentCopy3);
  System.out.println(inceptionCopy1);
  
 }
 
 public static void fillRegistry(NotebookRegistry notebookRegistry) {
  
  Notebook notebook1 = new Notebook();
  notebook1.setBookTitle("DIVERGENT");
  notebook1.setBookPrice(100);
  
  notebookRegistry.register(notebook1, Books.DIVERGENT.toString());
  
  Notebook notebook2 = new Notebook();
  notebook2.setBookTitle("INCEPTION");
  notebook2.setBookPrice(200);
  
  notebookRegistry.register(notebook2, Books.INCEPTION.toString());
  
 }
 
 
}

//we can directly use String, however, this can help in avoiding typos
enum Books{
 DIVERGENT,
 BRAVE,
 INCEPTION
}
```

### SAMPLE OUTPUT

The bookFact will change for each run.

```text
Notebook [bookTitle=DIVERGENT, bookPrice=100.0, bookFact=The Mona Lisa has no eyebrows.]
Notebook [bookTitle=DIVERGENT, bookPrice=100.0, bookFact=A group of flamingos is called a flamboyance.]
Notebook [bookTitle=DIVERGENT, bookPrice=100.0, bookFact=The Eiffel Tower in Paris was originally intended to be a temporary structure.]
Notebook [bookTitle=INCEPTION, bookPrice=200.0, bookFact=Giraffes have the same number of neck vertebrae as humans.]
```
