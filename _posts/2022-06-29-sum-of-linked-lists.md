---
layout: post
title: Sum of Linked Lists
date: 2022-06-29 15:04 +0530
author: "Gaurav Kumar"
tags: "java leetcode linkedlist"
categories: "linkedlist"
---

### PROBLEM DESCRIPTION

Given two numbers as LinkedList in reverseOrder (most significant digit as the right most digit), return a LinkedList (reversed) as the sum.

> Example:  
>
> LinkedListOne: 2 -> 4 -> 7 -> 1  
> LinkedListTwo: 9 -> 4 -> 5  
>
> Output:
> 1 -> 9 -> 2 -> 2  
> 1742 + 946 = 2291

[leetcode](https://leetcode.com/problems/add-two-numbers/description/)

### SOLUTION

```java
import java.util.*;

class Program {
  public static class LinkedList {
    public int value;
    public LinkedList next;

    public LinkedList(int value) {
      this.value = value;
      this.next = null;
    }
  }

  public LinkedList insertAtEnd(LinkedList ll, int value){
    
    if(ll == null){
      return new LinkedList(value);
    }

    LinkedList temp = ll;
    while(temp.next != null){
      temp = temp.next;
    }
    LinkedList node = new LinkedList(value);
    temp.next=node;
    return ll;
  }

  public LinkedList sumOfLinkedLists(LinkedList linkedListOne, LinkedList linkedListTwo) {

    LinkedList ans = null;

    LinkedList x = linkedListOne;
    LinkedList y = linkedListTwo;

    int carry = 0;
    
    while(x != null || y != null || carry != 0){ //since the linkedlist size can be different, we will need to keep adding until the longest one is complete. Also, after the last number, it's possible that the carry is > 0, in which case, we will need to add that digit also.

      int first = 0;
      int second = 0;
      
      if(x != null){
        first = x.value;
      }

      if(y != null){
        second = y.value;
      }

      int total = first+second+carry;
      int next = total%10; //the next digit will be sum of digits in the list at that place plus any carry from previous step
      carry = total/10; //the carry for next step will be total/10

      ans = insertAtEnd(ans, next);

      if(x != null){ //if it's null, we have covered all digits of first list
        x = x.next;
      }

      if(y != null){ //if it's null, we have covered all digits of second list
        y = y.next;
      }
      
    }

    return ans;
    
  }
}
```

### Another Way to Code

```java
class Solution {
    
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {

        ListNode dummyHead = new ListNode(0);
        ListNode tail = dummyHead;

        int carry = 0;

        while(l1 != null || l2 != null || carry != 0){
            
            int x = (l1==null?0:l1.val);
            int y = (l2==null?0:l2.val);

            int total = x+y+carry;

            tail.next = new ListNode(total%10);
            tail = tail.next;

            carry = total/10;

            if(l1 != null) l1 = l1.next;
            if(l2 != null) l2 = l2.next;

        }

        return dummyHead.next;

    }
}
```
