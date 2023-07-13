---
layout: post
title: 'Design Pattern: Factory'
date: 2023-07-13 22:24 +0530
author: "Gaurav Kumar"
tags: "java design_patterns"
categories: "design_patterns"
---

## FACTORY

This pattern involves creating two key components: a "product" and a "factory."  

In this example, the "product" is the Database interface. It defines the common methods that all types of databases should have, such as connect() and disconnect(). It also includes an additional method, createDatabaseFactory(), which is used to obtain a specific factory object related to the database.  

The Database interface has two implementations: MySQLDatabase and PostgreSQLDatabase. These classes provide the actual implementation of the connect() and disconnect() methods. Additionally, they both override the createDatabaseFactory() method to return the corresponding factory object (MySQLDatabaseFactory and PostgreSQLDatabaseFactory, respectively).  

Now, let's move on to the "factory" component. In this case, the "factory" is represented by the DatabaseFactory interface. It declares a single method, getQuery(), which is responsible for creating and returning a specific type of query object.  

The DatabaseFactory interface has two concrete implementations: MySQLDatabaseFactory and PostgreSQLDatabaseFactory. These classes implement the getQuery() method and return specific query objects (MySQLQuery and PostgresQuery, respectively).  

By using the Factory Method design pattern, we achieve a few benefits. Firstly, the client code only interacts with the Database interface, without needing to know the specific implementation details of MySQL or PostgreSQL. This provides abstraction and simplifies the usage for the client.  

Secondly, when a client wants to work with a particular database, they create an instance of the corresponding Database implementation (e.g., MySQLDatabase or PostgreSQLDatabase). This instance, in turn, provides the specific factory object related to the database via the createDatabaseFactory() method.  

The client can then use the factory object to create query objects (Query) without worrying about the details of object creation. The factories encapsulate the creation logic and handle the instantiation of specific query objects.  

This design allows for easy extensibility. If you want to support a new type of database, you would need to create a new implementation of the Database interface, along with its corresponding factory implementation. This way, the existing client code remains unaffected, and you can seamlessly add support for the new database type.  

Here's an example of how the code would look like:

## DATABASE INTERFACE

```java
package com.designpattern;

public interface Database {

    void connect();
    void disconnect();
    
    DatabaseFactory createDatabaseFactory();
 
}
```

## DATABASE FACTORY INTERFACE

```java
package com.designpattern;

public interface DatabaseFactory {
 
 Query getQuery();
 
}
```

## QUERY INTERFACE

```java
package com.designpattern;

public interface Query {
 
 void executeQuery();
 
}
```

## MYSQL QUERY AND POSTGRES QUERY IMPLEMENTATION

```java
package com.designpattern;

public class MySQLQuery implements Query{

 @Override
 public void executeQuery() {
  System.out.println("running mysql query");
 }

}
```

```java
package com.designpattern;

public class PostgresQuery implements Query{

 @Override
 public void executeQuery() {
  System.out.println("running postgres query");
 }


}
```

## MYSQL AND POSTGRES CONCRETE CLASSES

```java
package com.designpattern;

public class MySQLDatabase implements Database{

 @Override
 public void connect() {
  System.out.println("connecting to mysql db.");
 }

 @Override
 public void disconnect() {
  System.out.println("disconnecting mysql db.");
 }

 @Override
 public DatabaseFactory createDatabaseFactory() {
  return new MySQLDatabaseFactory();
 }

}
```

```java
package com.designpattern;

public class PostgreSQLDatabase implements Database{

 @Override
 public void connect() {
  System.out.println("connecting to postgres db.");
 }

 @Override
 public void disconnect() {
  System.out.println("disconnecting postgres db.");
  
 }

 @Override
 public DatabaseFactory createDatabaseFactory() {
  return new PostgreSQLDatabaseFactory();
 }

}
```

## MYSQL FACTORY AND POSTGRES FACTORY IMPLEMENTATION

```java
package com.designpattern;

public class MySQLDatabaseFactory implements DatabaseFactory{

 @Override
 public Query getQuery() {
  return new MySQLQuery();
 }


}
```

```java
package com.designpattern;

public class PostgreSQLDatabaseFactory implements DatabaseFactory{

 @Override
 public Query getQuery() {
  return new PostgresQuery();
 }

}
```

## CLIENT CODE

```java
package com.demo;

import com.designpattern.Database;
import com.designpattern.DatabaseFactory;
import com.designpattern.MySQLDatabase;
import com.designpattern.Query;

public class Client {

 public static void main(String[] args) {
  
  Database db = new MySQLDatabase();
  DatabaseFactory databaseFactory = db.createDatabaseFactory();
  
  db.connect();
  
  Query query = databaseFactory.getQuery();
  query.executeQuery();
  
 }
 
}
```

## SAMPLE OUTPUT

```text
connecting to mysql db.
running mysql query
```
