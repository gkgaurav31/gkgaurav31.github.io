---
layout: post
title: Find the Celebrity
date: 2023-10-18 20:31 +0530
author: "Gaurav Kumar"
tags: "java graphs leetcode leetcodealgo100"
categories: "graphs"
---

## PROBLEM DESCRIPTION

Suppose you are at a party with n people labeled from 0 to n - 1 and among them, there may exist one celebrity. The definition of a celebrity is that all the other n - 1 people know the celebrity, but the celebrity does not know any of them.

Now you want to find out who the celebrity is or verify that there is not one. You are only allowed to ask questions like: "Hi, A. Do you know B?" to get information about whether A knows B. You need to find out the celebrity (or verify there is not one) by asking as few questions as possible (in the asymptotic sense).

You are given a helper function bool knows(a, b) that tells you whether a knows b. Implement a function int findCelebrity(n). There will be exactly one celebrity if they are at the party.

Return the celebrity's label if there is a celebrity at the party. If there is no celebrity, return -1.

[leetcode](https://leetcode.com/problems/find-the-celebrity/)

## SOLUTION

### BRUTE-FORCE

```java
/* The knows API is defined in the parent class Relation.
   boolean knows(int a, int b); */

public class Solution extends Relation {

    public int findCelebrity(int n) {

        // Loop through each person from 0 to n-1
        for(int i = 0; i < n; i++){

            // Check if the current person (person1) might be a celebrity
            int person1 = i;

            // Assume person1 is a potential celebrity, and we'll try to verify it
            boolean found = true;

            // For each other person (person2) in the party
            for(int j = 0; j < n; j++){

                // Skip checking if a person is compared with themselves
                if(i == j) continue;

                int person2 = j;

                // If person1 knows person2, or person2 does not know person1,
                // person1 cannot be the celebrity, and we break the loop.
                if(knows(i, j) || !knows(j, i)){
                    found = false;
                    break;
                }
            }

            // If the loop finishes without breaking, and 'found' is still true,
            // person1 is a potential celebrity as they met the criteria.
            if(found) return person1;
        }

        // If no potential celebrity is found after checking all people, return -1.
        return -1;
    }
}
```

### OPTIMIZATION

The two important observations are:

- If knows(A, B) is true, then A cannot be a celebrity.
- If knows(A, B) is false, then B cannot be a celebrity (since everyone must know the celebrity)

These two observations can help in finding the last candidate for the celebrity. It's possible that there is no celebrity at all. So, before returning we need to double check if the last candidate is actually a celebrity, similar to the brute force way.

````java
/* The knows API is defined in the parent class Relation.
      boolean knows(int a, int b); */

public class Solution extends Relation {

    public int findCelebrity(int n) {

        int numberOfPeople = n;

        int possibleCelebrity = 0;

        for(int i=0; i<numberOfPeople; i++){

            // if current possible celebrity knows person i, current person cannot be a celeb
            // but the other person can be, so let's check that
            if(knows(possibleCelebrity, i)){
                possibleCelebrity = i;
            }
            //if possibleCeleb does not know i, we will continue with the current celeb only

        }

        // we will be left with a single candidate
        // it's possible that there are no celeb at all
        // so let's confirm that the current candidate is actually a celeb
        for(int i=0; i<numberOfPeople; i++){
            if(possibleCelebrity == i) continue;
            if(knows(possibleCelebrity, i) || !knows(i, possibleCelebrity)) return -1;
        }

        return possibleCelebrity;

    }

}
```

````
