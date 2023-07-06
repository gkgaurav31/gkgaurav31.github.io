---
layout: post
title: 'Design Pattern: Builder'
date: 2023-07-06 22:18 +0530
author: "Gaurav Kumar"
tags: "java design_patterns"
categories: "design_patterns"
---

## BUILDER

__Thought Process:__

- We need to create an object of a class which has numerous attributes and they need some kind of validation before we create the object
- Although we can directly use a constructor for this, the code will not be very readable or maintainable
- We could potentially use multiple constructors, however, not all combinations would be allowed. Example: new Student(fname, score) and new Student(fname, weight). If fname is String type, and score and weight are double, it will cause compile time error. There will also be a lot of code duplication.
- We will also assume here that the object needs to be immutable, which means that we cannot have any setter/getters or any other method which can modify the attributes.
- One option is to use a helper builder class to assign the required values to the attributes and use them to create the actual object.

### APPROACH 1

StudentBuilder class would have all the values initialized. This class does not need to be immutable.

```java
package designpattern;

public class StudentBuilder {

 String firstName;
 String lastName;
 int age;
 String gender;
 
}
```

Then, we can use this StudentBuilder class in the Student constructor while creating the object.

```java
package designpattern;

public class Student {

 private String firstName;
 private String lastName;
 private int age;
 private String gender;
 
 Student(StudentBuilder sb){
  sb.firstName = this.firstName;
  sb.lastName = this.lastName;
  sb.age = this.age;
  sb.gender = this.gender;
 }
 
}
```

The client mode might look like this:

```java
package designpattern;

public class Client {

 public static void main(String[] args) {
  
  StudentBuilder sb = new StudentBuilder();
  sb.firstName = "abc";
  sb.lastName = "def";
  sb.age = 20;
  sb.gender = "male";
  
  Student student = new Student(sb);
  
 }
 
}
```

There is one small problem that the client can send ```null``` while creating the new Student object, which is not an ideal scenario. To handle this, we will need to mark the Student constructor private. Also, it would also make sense for the Student class to return the StudentBuilder object (From the perspective of a client, otherwise they will need to somehow know that there exists another class called StudentBuilder while creating Student object).  
The other bigger problem is that the Student constructor will not be accessible from outside of the Student class once we make it private. So, we will need to "merge" Student and StudentBuilder class. This can be done in the following way:

### APPROACH 2

```java
package designpattern;

public class Student {

 private String firstName;
 private String lastName;
 private int age;
 private String gender;
 
 //mark the constructor private so that the object cannot be created from outside directly
 //this also avoid send null while creating student object
 private Student(StudentBuilder sb){
  sb.firstName = this.firstName;
  sb.lastName = this.lastName;
  sb.age = this.age;
  sb.gender = this.gender;
 }
 
 //return student builder direct from student class to make it easier for the clients
 public static StudentBuilder getStudentBuilder() {
  return new StudentBuilder();
 }
 
 //include StudentBuild class in Student class itself. We will have to mark it as static
 public static class StudentBuilder {

  String firstName;
  String lastName;
  int age;
  String gender;
  
  //create a build method to use the current student builder object to create the required student object and return it
  public Student build() {
   return new Student(this);
  }

  public String getFirstName() {
   return firstName;
  }

  public void setFirstName(String firstName) {
   this.firstName = firstName;
  }

  public String getLastName() {
   return lastName;
  }

  public void setLastName(String lastName) {
   this.lastName = lastName;
  }

  public int getAge() {
   return age;
  }

  public void setAge(int age) {
   this.age = age;
  }

  public String getGender() {
   return gender;
  }

  public void setGender(String gender) {
   this.gender = gender;
  }
  
 }
 
}
```

Client:

```java
package demo;

import designpattern.Student;

public class Client {

 public static void main(String[] args) {

  //get the student builder
  Student.StudentBuilder sb = Student.getStudentBuilder();
  
  //initialize student builder
  sb.setFirstName("abc");
  sb.setLastName("def");
  sb.setAge(20);
  sb.setGender("male");
  
  //build student object using the student builder object
  Student student = sb.build();
  
 }
 
}
```

The previous code fulfills all our requirements to create the Student object. However, we can make this even easier. It would be great, if the Client could do something like this while creating the Student object:

```java
Student student = Student.getStudentBuilder()
    .setFirstName("abc")
    .setLastName("def") // -> This will not work currently. The setFirstName return a void. What if, it could return StudentBuilder? That's the solution!
    .setAge(20)
    .setGender("male")
    .build();
```

### FINAL VERSION

- Change the return type of all the setters in StudentBuilder class to StudentBuilder.
- return the current object (so that it can be updated by the subsequent methods)

```java
package designpattern;

public class Student {

 private String firstName;
 private String lastName;
 private int age;
 private String gender;
 
 private Student(StudentBuilder sb){
  sb.firstName = this.firstName;
  sb.lastName = this.lastName;
  sb.age = this.age;
  sb.gender = this.gender;
 }
 
 public static StudentBuilder getStudentBuilder() {
  return new StudentBuilder();
 }
 
 public static class StudentBuilder {

  String firstName;
  String lastName;
  int age;
  String gender;
  
  public Student build() {
   return new Student(this);
  }

  public String getFirstName() {
   return firstName;
  }

  public StudentBuilder setFirstName(String firstName) {
   this.firstName = firstName;
   return this;
  }

  public String getLastName() {
   return lastName;
  }

  public StudentBuilder setLastName(String lastName) {
   this.lastName = lastName;
   return this;
  }

  public int getAge() {
   return age;
  }

  public StudentBuilder setAge(int age) {
   this.age = age;
   return this;
  }

  public String getGender() {
   return gender;
  }

  public StudentBuilder setGender(String gender) {
   this.gender = gender;
   return this;
  }
  
 }
 
}
```

Client:

```java
package demo;

import designpattern.Student;

public class Client {

 public static void main(String[] args) {

  Student student = Student.getStudentBuilder()
        .setFirstName("abc")
        .setLastName("def")
        .setAge(20)
        .setGender("male")
        .build();
  
 }
 
}
```
