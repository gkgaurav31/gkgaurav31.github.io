---
layout: post
title: Add Number Linked Lists (geeksforgeeks - SDE Sheet)
date: 2024-09-14 15:03 +0530
author: "Gaurav Kumar"
tags: "java linkedlist geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given two numbers, num1, and num2, represented by linked lists. The task is to return the head of the linked list representing the sum of these two numbers.

For example, the number 190 will be represented by the linked list, 1->9->0->null, similarly 25 by 2->5->null. Sum of these two numbers is 190 + 25 = 215, which will be represented by 2->1->5->null. You are required to return the head of the linked list 2->1->5->null.

Note: There can be leading zeros in the input lists, but there should not be any leading zeros in the output list.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/add-two-numbers-represented-by-linked-lists/1?page=6)

## SOLUTION

Reverse the given lists to make it simpler to evaluate the answer. Take one node at a time and calculate the current digit in the answer and carry. Stop when the digits exhaust and carry becomes 0.

```java
class Solution {

    static Node addTwoLists(Node num1, Node num2) {

        num1 = reverseLinkedList(num1);
        num2 = reverseLinkedList(num2);

        int carry = 0;

        Node list = null;

        Node h1 = num1;
        Node h2 = num2;

        while(h1 != null || h2 != null || carry != 0){

            int d1 = h1 == null ? 0 : h1.data;
            int d2 = h2 == null ? 0 : h2.data;

            int sum = d1 + d2 + carry;

            int nextDigit = sum%10;
            carry = sum/10;

            // add node
            Node newNode = new Node(nextDigit);
            newNode.next = list;
            list = newNode;

            if(h1 != null) h1 = h1.next;
            if(h2 != null) h2 = h2.next;

        }

        // restore original lists
        num1 = reverseLinkedList(num1);
        num2 = reverseLinkedList(num2);

        return list;

    }

    static Node reverseLinkedList(Node head){

        Node h1 = head;
        Node h2 = null;

        while(h1 != null){
            Node t = h1;
            h1 = h1.next;
            t.next = h2;
            h2 = t;
        }

        return h2;
    }

}
```
