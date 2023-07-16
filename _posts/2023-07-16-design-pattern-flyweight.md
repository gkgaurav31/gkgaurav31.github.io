---
layout: post
title: 'Design Pattern: Flyweight'
date: 2023-07-16 18:39 +0530
author: "Gaurav Kumar"
tags: "java design_patterns"
categories: "design_patterns"
---

## FLYWEIGHT

The Flyweight pattern is a design pattern that helps optimize memory usage by sharing common data between multiple objects. It is particularly useful when dealing with objects that have both __intrinsic (shared)__ and __extrinsic (unique)__ attributes.  

In this example, let's consider the Ball class with the attributes color, imageURL, coordinateX, coordinateY, and radius. The color and imageURL can be considered intrinsic attributes because they are the same for all balls of the same type in the game. On the other hand, the coordinateX, coordinateY, and radius are extrinsic attributes because they vary for each individual ball object.

[Good Explanation by Yogita Sharma - YouTube](https://www.youtube.com/watch?v=8cL9KbHS5kE)

## OVERVIEW (UML)

![snapshot]({{ site.baseurl }}/assets/img/design_patterns/flyweight.png)
*Created using [PlantUML](https://plantuml.com/).*

## CODE WALKTHROUGH

### STORING THE INTRINSIC ATTRIBUTES USING FLYWEIGHT

```java
package com.designpattern;

//Flyweight ball class representing shared intrinsic attributes
//These attributes will not change for given "ballColor" and "ballImageURL" (so, no setter methods)
public class FlyweightBall {

 private String ballColor;
 private String ballImageURL;
 
 public FlyweightBall(String ballColor, String ballImageURL) {
  this.ballColor = ballColor;
  this.ballImageURL = ballImageURL;
 }

 public String getBallColor() {
  return ballColor;
 }

 public String getBallImageURL() {
  return ballImageURL;
 }

 public void setBallColor(String ballColor) {
  this.ballColor = ballColor;
 }

 public void setBallImageURL(String ballImageURL) {
  this.ballImageURL = ballImageURL;
 }
 
}
```

### USING FLYWEIGHT OBJECT IN THE ACTUAL CLASS

```java
package com.designpattern;

//Ball class representing individual balls with unique extrinsic attributes
//This will have getters and setters for values which are dynamic
public class Ball {

 //reference to the intrinsic values
 private FlyweightBall flyweightBall;
 
    private int coordinateX;
    private int coordinateY;
    private int radius;
    
    //constructor to set the intrinsic value
    //every ball must have some value for X and Y coordinates, so we can set them using constructor itself here
    public Ball(FlyweightBall flyweightBall, int coordinateX, int coordinateY) {
        this.flyweightBall = flyweightBall;
        this.coordinateX = coordinateX;
        this.coordinateY = coordinateY;
    }

 public FlyweightBall getFlyweightBall() {
  return flyweightBall;
 }

 public void setFlyweightBall(FlyweightBall flyweightBall) {
  this.flyweightBall = flyweightBall;
 }

 public int getCoordinateX() {
  return coordinateX;
 }

 public void setCoordinateX(int coordinateX) {
  this.coordinateX = coordinateX;
 }

 public int getCoordinateY() {
  return coordinateY;
 }

 public void setCoordinateY(int coordinateY) {
  this.coordinateY = coordinateY;
 }

 public int getRadius() {
  return radius;
 }

 public void setRadius(int radius) {
  this.radius = radius;
 }

 @Override
 public String toString() {
  return "Ball [flyweightBall=" + flyweightBall + ", coordinateX=" + coordinateX + ", coordinateY=" + coordinateY
    + ", radius=" + radius + "]";
 }
 
}
```

### USING FACTORY CLASS TO MANAGE FLYWEIGHT OBJECTS

The BallFactory class acts as a cache for managing FlyweightBall objects. It ensures that only one instance of a FlyweightBall with a specific combination of ballColor and ballImageURL is created. This prevents the unnecessary creation of multiple FlyweightBall objects with the same intrinsic attributes and helps save memory.  

By reusing the same FlyweightBall object for different balls, the Flyweight pattern helps minimize memory consumption, especially when dealing with a large number of balls that share common intrinsic attributes.

```java
package com.designpattern;

import java.util.HashMap;
import java.util.Map;

//BallFactory class for creating and managing FlyweightBall objects
//This is like a cache. The "FlyweightBall" for a given "ballColor" and "ballImageURL" will not change
//So, it checks it an object with a given "ballColor" and "ballImageURL" already exists. If so, return it. 
//This is where we are saving memory because for thousands of balls, we are referring to the same Flyweight object
public class BallFactory {

 private static Map<String, FlyweightBall> ballPool = new HashMap<>();

 public static FlyweightBall getFlyweightBall(String color, String imageURL) {
  
  String key = color + "_" + imageURL;

  // Check if a flyweight ball with the same intrinsic attributes already exists
  if (ballPool.containsKey(key)) {
   return ballPool.get(key);
  } else {
   // If not, create a new flyweight ball and add it to the pool
   FlyweightBall flyweightBall = new FlyweightBall(color, imageURL);
   ballPool.put(key, flyweightBall);
   return flyweightBall;
  }
 }

}
```

### CLIENT

```java
package com.demo;

import com.designpattern.Ball;
import com.designpattern.BallFactory;

public class App {

 public static void main(String[] args) {
   
  //this will create a new flyweight ball object for key = color + "_" + imageURL;
  Ball redBall1 = new Ball(BallFactory.getFlyweightBall("red", "redball.png"), 100, 200);
  //since key = color + "_" + imageURL will be the same, it will re-use the existing flyweight object
  Ball redBall2 = new Ball(BallFactory.getFlyweightBall("red", "redball.png"), 200, 300);

  //we can set other extrinsic properties like the radius by calling the setter method.
  
  System.out.println("BALL 1:");
  System.out.println(redBall1);
  System.out.println(redBall1.getFlyweightBall().getBallColor());
  System.out.println(redBall1.getFlyweightBall().getBallImageURL());
  
  
  System.out.println();
  
  System.out.println("BALL 2:");
  System.out.println(redBall2);
  System.out.println(redBall2.getFlyweightBall());
  System.out.println(redBall2.getFlyweightBall().getBallColor());
  System.out.println(redBall2.getFlyweightBall().getBallImageURL());
  
 }
 
}
```

### OUTPUT

Notice here that the flyweighBall has the same reference!

```text
BALL 1:
Ball [flyweightBall=com.designpattern.FlyweightBall@4c203ea1, coordinateX=100, coordinateY=200, radius=0]
red
redball.png

BALL 2:
Ball [flyweightBall=com.designpattern.FlyweightBall@4c203ea1, coordinateX=200, coordinateY=300, radius=0]
com.designpattern.FlyweightBall@4c203ea1
red
redball.png
```
