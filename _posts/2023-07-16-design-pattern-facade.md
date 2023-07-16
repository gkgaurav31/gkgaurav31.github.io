---
layout: post
title: 'Design Pattern: Facade'
date: 2023-07-16 13:47 +0530
author: "Gaurav Kumar"
tags: "java design_patterns"
categories: "design_patterns"
---

## FACADE

The Facade design pattern is a structural design pattern that provides a simplified interface to a complex system, making it easier to use. It encapsulates the complexity of the system behind a single class, known as the facade, that exposes a simple and unified interface to the client.  

Let's consider a simple example of a movie theater system. The theater system consists of multiple subsystems such as the ticket booking system, the snack bar, and the movie playback system. Each subsystem has its own set of classes and methods.

## SUBSYSTEMS IN A MOVIE THEATRE

### TICKET BOOKING SYSTEM

```java
package com.designpattern;

class TicketBookingSystem {
 
    public void bookTicket(String movieName, int numberOfTickets) {
        System.out.println("Booking " + numberOfTickets + " tickets for " + movieName);
    }
    
}
```

### SNACKS COUNTER

```java
package com.designpattern;

import java.util.Arrays;

class SnackBar {
    public void orderSnacks(String[] items) {
        System.out.println("Ordering snacks: " + Arrays.toString(items));
    }
}
```

### MOVIE PLAYBACK SYSTEM

```java
package com.designpattern;

class MoviePlaybackSystem {
 
    public void playMovie(String movieName) {
        System.out.println("Playing movie: " + movieName);
    }
    
}
```

## MOVIE THEATRE FACADE: COMBINING IT ALL

In this example, the MovieTheaterFacade class acts as the facade. It encapsulates the ticket booking system, the snack bar, and the movie playback system behind a simplified method watchMovie(). The watchMovie() method takes the movie name, the number of tickets, and an array of snacks as parameters.

When the client code calls movieTheaterFacade.watchMovie(), it doesn't need to know about the internal workings of the subsystems. The facade takes care of coordinating the actions of the subsystems, such as booking tickets, ordering snacks, and playing the movie.

The Facade design pattern provides a simplified interface to the complex movie theater system, making it easier for the client to interact with. It hides the complexity and details of the subsystems, promoting a more straightforward and intuitive usage.

```java
package com.designpattern;

//Facade Class
class MovieTheaterFacade {
 
 private TicketBookingSystem ticketBookingSystem;
 private SnackBar snackBar;
 private MoviePlaybackSystem moviePlaybackSystem;

 public MovieTheaterFacade(TicketBookingSystem ticketBookingSystem, SnackBar snackBar,
   MoviePlaybackSystem moviePlaybackSystem) {
  
  this.ticketBookingSystem = ticketBookingSystem;
  this.snackBar = snackBar;
  this.moviePlaybackSystem = moviePlaybackSystem;
  
 }

 // Facade method for booking tickets, ordering snacks, and playing the movie
 public void watchMovie(String movieName, int numberOfTickets, String[] snacks) {
  ticketBookingSystem.bookTicket(movieName, numberOfTickets);
  snackBar.orderSnacks(snacks);
  moviePlaybackSystem.playMovie(movieName);
 }
 
}
```

## CLIENT

```java
package com.designpattern;

public class App {

 public static void main(String[] args) {
  
  // Create instances of the subsystems
        TicketBookingSystem ticketBookingSystem = new TicketBookingSystem();
        SnackBar snackBar = new SnackBar();
        MoviePlaybackSystem moviePlaybackSystem = new MoviePlaybackSystem();

        // Create a facade instance and inject the subsystem instances
        MovieTheaterFacade movieTheaterFacade = new MovieTheaterFacade(ticketBookingSystem, snackBar, moviePlaybackSystem);

        // Use the facade to watch a movie
        String movieName = "Avengers: Endgame";
        int numberOfTickets = 2;
        String[] snacks = { "Popcorn", "Soda" };
        
        movieTheaterFacade.watchMovie(movieName, numberOfTickets, snacks);
        
 }
 
}
```

### OUTPUT

```text
Booking 2 tickets for Avengers: Endgame
Ordering snacks: [Popcorn, Soda]
Playing movie: Avengers: Endgame
```
