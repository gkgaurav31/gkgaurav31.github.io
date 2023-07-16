---
layout: post
title: 'Design Pattern: Decorator'
date: 2023-07-16 01:27 +0530
author: "Gaurav Kumar"
tags: "java design_patterns"
categories: "design_patterns"
---

## DECORATOR

Decorator design pattern is a __structural design pattern__ that allows us to add extra features or behaviors to an object without changing its original structure.

In this example, we have a Pizza class which represents the core functionality of a pizza. It has a getDescription() method that returns the description of the pizza and a getPizzaCost() method that returns the cost of the pizza.

Now, let's say we want to add extra toppings or decorations to our pizza, such as tomatoes and cheese. Instead of modifying the Pizza class directly, we use the Decorator pattern to dynamically add these toppings.

We create a PizzaDecorator class that extends the Pizza class. This decorator has a reference to another Pizza object. It acts as a wrapper around the decorated pizza. The decorator also has the same methods as the Pizza class, but it modifies the behavior by adding extra functionality.

In our example, we have concrete decorator classes: TomatoDecorator and CheeseDecorator. The TomatoDecorator adds the functionality of tomatoes to the pizza, and the CheeseDecorator adds the functionality of cheese. These decorator classes extend the PizzaDecorator class and modify the getDescription() and getPizzaCost() methods to include the respective toppings.

To create a decorated pizza, we can chain the decorators together. For example, we can create a pizza with both tomatoes and cheese by wrapping a Margherita pizza with a TomatoDecorator and a CheeseDecorator. The decorators add the toppings and modify the behavior accordingly.

## OVERVIEW (UML)

![snapshot]({{ site.baseurl }}/assets/img/design_patterns/decorator.png)
*Created using [PlantUML](https://plantuml.com/).*

## CODE WALKTHROUGH

### BASE COMPONENT (PIZZA)

```java
package com.designpattern;

public abstract class Pizza {
 
 private double pizzaCost;
 
    public abstract String getDescription();
    
 public double getPizzaCost() {
  return pizzaCost;
 }
 public void setPizzaCost(double pizzaCost) {
  this.pizzaCost = pizzaCost;
 }
 
}
```

### PIZZA DECORATOR FOR THE TOPPINGS

The PizzaDecorator class is an important part of the Decorator design pattern. It serves as a base for all concrete decorators and provides a way to __wrap__ and decorate the core pizza object.  

Why does PizzaDecorator have a Pizza and also extend Pizza?  

The PizzaDecorator has a reference to another Pizza object because it needs to wrap and decorate that pizza.  

By extending the Pizza class itself, the PizzaDecorator class ensures that it is treated as a type of Pizza. This allows us to use decorators interchangeably with the core pizza object.  

In simpler terms, the PizzaDecorator is like a container that holds a pizza and adds extra functionality or decorations to it. It can be used as if it were a pizza itself, even though it's just enhancing the existing pizza. This idea let's us build the Pizza with Toppings layer by layer.

```java
package com.designpattern;

//is-a Pizza
public abstract class PizzaDecorator extends Pizza{

 //has-a pizza
 public Pizza pizza;
 
 public PizzaDecorator(Pizza pizza) {
  this.pizza = pizza;
 }

}
```

### BASE PIZZA CONCRETE CLASSES

#### Margherita Pizza

```java
package com.designpattern;

public class Margherita extends Pizza{

 public Margherita() {
  setPizzaCost(100.0);
 }
 
 @Override
 public String getDescription() {
  return "Margarita Pizza";
 }

}
```

#### Plain Pizza

```java
package com.designpattern;

public class PlainPizza extends Pizza{

 public PlainPizza() {
  setPizzaCost(80.0);
 }
 
 @Override
 public String getDescription() {
  return "Plain Pizza";
 }
 
}
```

### PIZZA DECORATOR CONCRETE CLASSES

__IMPORTANT:__ The ```getPizzaCost()``` method of the decorator considers the cost of both the core pizza and any decorations, while the getPizzaCost() method of the plain pizza or Margherita pizza only returns the base cost of the pizza itself.  

In the decorator pattern, the decorators wrap around the core pizza object and provide additional features or decorations. These additional features may have an impact on the cost of the pizza. Therefore, when we call getPizzaCost() on a decorated pizza, the decorator's implementation of getPizzaCost() is invoked, which includes the cost of the decorations it adds.

#### CHEESE DECORATOR

```java
package com.designpattern;

public class CheeseDecorator extends PizzaDecorator {

 public CheeseDecorator(Pizza pizza) {
  super(pizza);
 }

 @Override
 public String getDescription() {
  return pizza.getDescription() + "; Cheese";
 }

 @Override
 public double getPizzaCost() {
  return pizza.getPizzaCost() + 20.0;
 }

}
```

#### TOMATO DECORATOR

```java
package com.designpattern;

public class TomatoDecorator extends PizzaDecorator{

 public TomatoDecorator(Pizza pizza) {
  super(pizza);
 }

 @Override
 public String getDescription() {
  return pizza.getDescription() + "; tomato";
 }
 
 @Override
 public double getPizzaCost() {
  return pizza.getPizzaCost() + 10.0;
 }
}
```

### CLIENT

```java
package com.designpattern;

public class App {

 public static void main(String[] args) {
  
  Pizza pizza = new TomatoDecorator(new CheeseDecorator(new Margherita()));
  
  System.out.println(pizza.getPizzaCost());
  System.out.println(pizza.getDescription());
  
 }
 
}
```

### OUTPUT

```text
130.0
Margarita Pizza, Cheese; tomato
```
