---
layout: post
title: 'Design Pattern: Observer'
date: 2023-07-17 22:04 +0530
author: "Gaurav Kumar"
tags: "java behavioral_design_patterns design_patterns"
categories: "design_patterns"
---

## OBSERVER

The Observer design pattern is a behavioral design pattern that establishes a one-to-many dependency between objects, so that when one object changes its state, all the dependent objects are automatically notified and updated. This is similar to a pub/sub model. The Subject is the one which needs to be observed by multiple Observers. When something changes in the subject, it notifies all the observers which can take appropriate actions.

## OVERVIEW (UML)

![snapshot]({{ site.baseurl }}/assets/img/design_patterns/observer.png)
*Created using [PlantUML](https://plantuml.com/).*

Let's imagine a scenario where we have a weather station that collects weather data (such as temperature, humidity, and pressure) and wants to notify multiple displays when the weather conditions change. To implement the Observer design pattern, we'll create three main components: the Subject, the Observers, and the Concrete Subject.

## CODE WALKTHROUGH

### SUBJECT INTERFACE - THE THING TO BE OBSERVED

This is the interface that defines the methods for registering, removing, and notifying observers.

```java
package com.designpattern;

public interface Subject {

 void registerObserver(Observer observer);
 void removeObserver(Observer observer);
 void notifyObservers();
 
}

```

### OBSERVER - THE ONES WHICH NEED TO BE NOTIFIED WHEN THE SUBJECT CHANGES

This is the interface that defines the method to be called when the subject notifies the observers.

```java
package com.designpattern;

public interface Observer {

 void update(float temperature, float humidity, float pressure);
 
}

```

### WEATHER STATION - SUBJECT IMPLEMENTATION

This is the class that implements the Subject interface and holds the current weather data.

```java
package com.designpattern;

import java.util.ArrayList;
import java.util.List;

public class WeatherStation implements Subject {

 List<Observer> observers;
 private float temperature;
 private float humidity;
 private float pressure;

 public WeatherStation() {
  observers = new ArrayList<Observer>();
 }

 @Override
 public void registerObserver(Observer observer) {
  observers.add(observer);
 }

 @Override
 public void removeObserver(Observer observer) {
  observers.remove(observer);
 }

 @Override
 public void notifyObservers() {

  //Iterate over the list of observers and calls their update() method.
  for (Observer observer : observers) {
   observer.update(temperature, humidity, pressure);
  }

 }
 
 //update the measurements and call the notifyObserver() method
 public void setMeasurements(float temperature, float humidity, float pressure) {
  
  this.temperature = temperature;
  this.humidity = humidity;
  this.pressure = pressure;
  
  notifyObservers();
  
 }

}
```

### DISPLAY - OBSERVER IMPLEMENTATION

This will mimic an observer which needs to be notified when the subject (WeatherStation) get an update, in this case, the method setMeasurement() is called.

```java
package com.designpattern;

public class Display implements Observer{

 private String displayName;
 
 public Display(String displayName) {
  this.displayName = displayName;
 }
 
 @Override
 public void update(float temperature, float humidity, float pressure) {
  
  System.out.println("Notified: " + displayName);
  
        System.out.println("Temperature: " + temperature);
        System.out.println("Humidity: " + humidity);
        System.out.println("Pressure: " + pressure);
        System.out.println();
  
 }

}
```

### CLIENT

```java
package com.demo;

import com.designpattern.Display;
import com.designpattern.WeatherStation;

public class App {

 public static void main(String[] args) {

  WeatherStation weatherStation = new WeatherStation();

  Display display1 = new Display("display1");
  Display display2 = new Display("display2");

  weatherStation.registerObserver(display1);
  weatherStation.registerObserver(display2);

  weatherStation.setMeasurements(25.5f, 65.2f, 1013.2f);
  
  weatherStation.removeObserver(display1);
  weatherStation.setMeasurements(25.0f, 75.2f, 1017.2f);

 }

}
```

### OUTPUT

```text
Notified: display1
Temperature: 25.5
Humidity: 65.2
Pressure: 1013.2

Notified: display2
Temperature: 25.5
Humidity: 65.2
Pressure: 1013.2

Notified: display2
Temperature: 25.0
Humidity: 75.2
Pressure: 1017.2
```
