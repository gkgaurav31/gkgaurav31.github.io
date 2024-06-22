---
layout: post
title: Merge Two Sorted Linked List - 2
date: 2022-09-08 23:10 +0530
author: "Gaurav Kumar"
tags: "java leetcode linkedlist"
categories: "linkedlist"
---

### PROBLEM DESCRIPTION

Merge two sorted Linked List.

[leetcode](https://leetcode.com/problems/merge-two-sorted-lists/description/)

### SOLUTION

![snapshot]({{ site.baseurl }}/assets/img/merge_sorted_linked_list.png)

```java
public static Node mergeSortedLinkedList(Node h1, Node h2){

    //If any of the list is empty, return the other sorted list
    if(h1 == null) return h2;
    if(h2 == null) return h1;

    Node t=null, h3=null;

    //Initialize
    if(h1.value < h2.value){
        t=h1;
        h3=h1;
        h1=t.next;
    }else{
        t=h2;
        h3=h2;
        h2=t.next;
    }


    while(h1 != null && h2 != null){
        if(h1.value < h2.value){
            t.next=h1;
            h1=h1.next;
            t=t.next;
        }else{
            t.next=h2;
            h2=h2.next;
            t=t.next;
        }
    }

    if(h1 == null){
        t.next = h2;
    }else{
        t.next = h1;
    }

    return h3;
}
```

### RECURSIVE SOLUTION

```java
class Solution {

    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {

        if(list1 == null && list2 == null) return null;
        if(list1 == null) return list2;
        if(list2 == null) return list1;

        ListNode h1=list1;
        ListNode h2=list2;

        ListNode h3;

        if(h1.val <= h2.val){
            h3=h1;
            ListNode t = h1.next;
            h1.next = mergeTwoLists(t, h2);
        }else{
            h3=h2;
            ListNode t = h2.next;
            h2.next = mergeTwoLists(h1, t);
        }

        return h3;

    }

}
```

### BETTER RECURSIVE CODE

```java
class Solution {

    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {

        if(list1 == null)
            return list2;

        if(list2 == null)
            return list1;

        if(list1.val < list2.val){
            list1.next = mergeTwoLists(list1.next, list2);
            return list1;
        }else{
            list2.next = mergeTwoLists(list1, list2.next);
            return list2;
        }

    }

}
```

### ANOTHER WAY TO CODE USING DUMMY HEAD

```java
class Solution {

    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {

        ListNode dummyHead = new ListNode(0);

        ListNode temp = dummyHead;

        while(list1 != null && list2 != null){

            int x = list1.val;
            int y = list2.val;

            if(x<y){
                temp.next = list1;
                list1 = list1.next;
            }else{
                temp.next = list2;
                list2 = list2.next;
            }

            temp = temp.next;

        }

        if(list1 != null){
            temp.next = list1;
        }else{
            temp.next = list2;
        }

        return dummyHead.next;

    }

}
```

### YET WAY TO CODE USING DOUBLY ENDED QUEUE (DEQUEUE)

```java
class Solution {

    public void reorderList(ListNode head) {

        //create a deque to store the nodes of the linked list
        Deque<ListNode> q = new LinkedList<>();

        // Temporary pointer to traverse the list
        // Traverse the list and add each node to the deque
        ListNode t = head;
        while (t != null) {
            q.add(t);
            t = t.next;
        }

        // Tail pointer to keep track of the last node processed in the reordered list
        ListNode tail = null;

        // Reorder the list by alternating nodes from the front and back of the deque
        while (q.size() >= 2) {

            // Remove the first node from the front of the deque
            ListNode first = q.removeFirst();

            // Remove the last node from the back of the deque
            ListNode last = q.removeLast();

            // Get the next node to be linked (if the deque is not empty, get the front node, otherwise null)
            ListNode next = (q.isEmpty() ? null : q.getFirst());

            // Link the first node to the last node
            first.next = last;

            // Link the last node to the next node
            last.next = next;

            // Update the tail pointer to the next node
            tail = next;

        }

        // If the tail is not null, set its next pointer to null to terminate the list
        // If we do not do this, if there are odd number of nodes the last node remaining in the dequeue (the middle node in the list) will lead to a cycle
        if (tail != null) {
            tail.next = null;
        }
    }
}
```
