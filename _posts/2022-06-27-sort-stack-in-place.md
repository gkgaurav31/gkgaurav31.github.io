---
layout: post
title: Sort Stack (In Place)
date: 2022-06-27 16:41 +0530
author: "Gaurav Kumar"
tags: "java stack important"
categories: "stack"
---

### PROBLEM DESCRIPTION

Given a stack, sort the elements in-place. (smallest element at the bottom of the stack)

### SOLUTION

Think about: If we are given an already sorted stack and we need to add an element such that the complete stack is sorted. If the value is greater than the element at the top, then we can simply add our element. But, if it's smaller, then we will need to keep "popping" the elements from the stack, until we reach the point in which our current element to be added is larger than the top of the stack.  

```java
import java.util.*;

class Program {

  public ArrayList<Integer> sortStack(ArrayList<Integer> stack) {

    if(stack.size() <= 0) return stack; //if the stack is empty, return it.

    int top = stack.remove(stack.size()-1); 

    sortStack(stack); //for each recursive stack call, it will "save" the value of top above. After this call, all elements will be popped out

    insertInSortedOrder(stack, top); //now we need to add the top element to the stack, while keep it sorted

    return stack;
    
  }

  public void insertInSortedOrder(ArrayList<Integer> stack, int value){
    
    if(stack.size() == 0 || (stack.get(stack.size()-1) <= value)){ //if the stack is empty or the value to be added is larger, add that to the top of the stack
      stack.add(value);
      return;
    }

    //if the element to be added is smaller, we will need to find the right place to add it. For that we will again keep to keep removing the top most element until there is an element at the top of the stack which is smaller than our element 
    int top = stack.remove(stack.size()-1); 
    insertInSortedOrder(stack, value); 

    stack.add(top);
    
  }
}
```

Similar logic, but another way to code:

```java
import java.util.*;

class Program {

  public ArrayList<Integer> sortStack(ArrayList<Integer> stack) {

    if(stack.size() <= 0) return stack;

    int top = stack.remove(stack.size()-1);

    sortStack(stack);

    if(stack.size() >=1 && stack.get(stack.size()-1) > top){
      
      int large = stack.remove(stack.size()-1);
      stack.add(top);
      stack.add(large);
      
      sortStack(stack); //This line does similar thing as the insertInSortedOrder method in the previous code.
      
    }else{
      stack.add(top);
    }

    return stack;
    
  }
}
```
