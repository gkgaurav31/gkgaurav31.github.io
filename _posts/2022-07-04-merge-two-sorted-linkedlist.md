---
layout: post
title: Merge Two Sorted LinkedList
date: 2022-07-04 23:44 +0530
author: "Gaurav Kumar"
tags: "java linkedlist important"
categories: "linkedlist"
---

### PROBLEM DESCRIPTION

Merge two LinkedList which are sorted such that the final LinkedList is also sorted. (Without creating a new LinkedList)

### SOLUTION

This problem is also part of AlgoExpert: [https://www.algoexpert.io/questions/merge-linked-lists](https://www.algoexpert.io/questions/merge-linked-lists)

![snapshot]({{ site.baseurl }}/assets/img/merge_linked_list.jpg)

The idea is to first decide which linkedlist we are going to modify to represent the final linked list. Then we keep adding elements from the other list to this list. The way we do it is, we keep moving foward on the first list. If the node in the 2nd list has a value less than the current node, then we add that node before the current node in list1. If it's greater, then we keep moving forward. Since we need to add the element from list2 behind the current node of list1, we also track the previous node in list1.

```java
import java.util.*;

class Program {

  public static LinkedList mergeLinkedLists(LinkedList headOne, LinkedList headTwo) {

    LinkedList p = null, t1 = headOne, t2 = headTwo;

    while(t1 != null && t2 != null){

      if(t2.value <= t1.value){
        //t2 should come before t1

        if(p==null){
          //edge-case when p is null (first update)
          p = t2;
          t2 = t2.next;
          p.next = t1;
        }else{
          p.next = t2;
          p = t2;
          t2 = t2.next;
          p.next = t1;
        }
        
      }else{
        p = t1;
        t1 = t1.next;
      }
      
    }

    //end of first list, but second list has more nodes
    //if the opposite happens, we will already have a sorted list
    if(t1 == null && t2 != null){
      p.next = t2;
    }

    //return the head node which has lower value since that will be the start of our final sorted list
    if(headOne.value < headTwo.value){
      return headOne;
    }else
      return headTwo;
    
  }
}
```
