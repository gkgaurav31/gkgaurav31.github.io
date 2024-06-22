---
layout: post
title: Reorder List
date: 2022-12-14 23:11 +0530
author: "Gaurav Kumar"
tags: "java leetcode linkedlist neetcode150"
categories: "neetcode150"
---

### PROBLEM DESCRIPTION

You are given the head of a singly linked-list. The list can be represented as:

> L0 → L1 → … → Ln - 1 → Ln

Reorder the list to be on the following form:

> L0 → Ln → L1 → Ln - 1 → L2 → Ln - 2 → …

You may not modify the values in the list's nodes. Only nodes themselves may be changed.

### SOLUTION

One way to solve this problem is to take a node and update it's next to reverse of remaining list. Do this recursively for each node.

### APPROACH 1

```java
class Solution {

    public void reorderList(ListNode head) {
        reorderListHelper(head);
    }

    public ListNode reorderListHelper(ListNode head){
        if(head == null) return null;
        head.next = reverseList(head.next);
        reorderListHelper(head.next);
        return head;
    }

    public ListNode reverseList(ListNode head) {

        ListNode h1=head;
        ListNode t=head;

        ListNode h2 = null;

        while(h1!=null){
            h1=h1.next;
            t.next=h2;
            h2=t;
            t=h1;
        }

        return h2;

    }

}
```

### APPROACH 2

```java
class Solution {

    public ListNode getCenter(ListNode head){

        ListNode slow = head;
        ListNode fast = head;

        while(fast != null && fast.next != null){
            slow = slow.next;
            fast = fast.next.next;
        }

        return slow;

    }

    public ListNode reverseLinkedList(ListNode head){

        ListNode h1 = head;
        ListNode h2 = null;
        ListNode t = head;

        while(h1 != null){
            h1 = h1.next;
            t.next = h2;
            h2 = t;
            t = h1;
        }

        return h2;

    }

    public ListNode mergeAlternative(ListNode h1, ListNode h2){

        if(h1 == null && h2 == null) return null;
        if(h1 == null && h2 != null) return h2;
        if(h1 != null && h2 == null) return h1;

        //save the next nodes of both lists
        ListNode t1 = h1.next;
        ListNode t2 = h2.next;

        //next node of front node of list1 should point to front node of list2
        h1.next = h2;
        //next node of front node of list2 should point to mergeAlternative(t1, t2) -- recursion
        h2.next = mergeAlternative(t1, t2);

        return h1;

    }

    public void breakList(ListNode head, ListNode center){
        ListNode temp = head;
        while(temp.next != center){
            temp = temp.next;
        }
        temp.next = null;
    }

    public void reorderList(ListNode head) {

        //edge case
        if(head.next == null) return;

        //get the center of the list
        //if the length is even, pick the second one
        ListNode center = getCenter(head);

        //break link to divide the list into two parts
        breakList(head, center);

        //reverse 2nd part
        ListNode h2 = reverseLinkedList(center);

        //merge them as required in the question by alternative nodes between the lists
        head = mergeAlternative(head, h2);

    }
}
```

### APPROACH 3 - USING DOUBLY ENDED QUEUE (DEQUEUE)

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
