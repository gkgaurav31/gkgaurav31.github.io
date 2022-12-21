---
layout: post
title: Merge K Sorted Linked Lists
date: 2022-09-10 23:53 +0530
author: "Gaurav Kumar"
tags: "java linkedlist important"
---

### PROBLEM DESCRIPTION

You are given the head of K sorted Linked Lists. Every Node is defined as:

```java
class Node{

    int data;
    Node right;
    Node down;
    Node(int x){
        data=x;
        right=null;
        right=null;
    }

}
```

The head node's down pointer, points to the head of next Linked List.  

Return a single Linked List which contains all the Nodes from the K list, in a sorted order.

### SOLUTION

![snapshot]({{ site.baseurl }}/assets/img/merge_k_sorted_linked_lists.png)

```java
Node mergeKSorted(Node h1){
    
    if(h1 == null || h1.down == null) return h1;

    Node c = center(h1); //Find center of the LinkedList using down pointer. 
    Node h2 = c.down;
    c.down = null;

    h1 = mergeKSorted(h1);
    h2 = mergeKSorted(h2);

    return merge(h1, h2); //[Merge Two Sorted Linked Lists]({% post_url 2022-09-09-merge-sort-linked-list %})


}
```

Related problem on leetcode: [https://leetcode.com/problems/merge-k-sorted-lists/](https://leetcode.com/problems/merge-k-sorted-lists/)

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {

    public ListNode mergeKLists(ListNode[] lists) {
        
        if(lists.length == 0) return null;
        
        if(lists.length <= 1) return lists[0];
        
        int c = center(lists);
        
        int s2 = c+1;
        
        ListNode l1 = mergeKLists(subArray(lists,0,c-1));
        ListNode l2 = mergeKLists(subArray(lists, c, lists.length-1));
        
        ListNode head = mergeTwoLists(l1, l2);
        
        return head;
        
    }

    public static<T> T[] subArray(T[] array, int beg, int end) {
        return Arrays.copyOfRange(array, beg, end + 1);
    }
    
    int center(ListNode[] lists){
        return lists.length/2;
    }
    
    public ListNode mergeTwoLists(ListNode A, ListNode B) {

            ListNode h1 = A;
            ListNode h2 = B;

            //If any of the list is empty, return the other sorted list
            if(h1 == null) return h2;
            if(h2 == null) return h1;

            ListNode t=null, h3=null;

            //Initialize
            if(h1.val < h2.val){
                t=h1;
                h3=h1;
                h1=t.next;
            }else{
                t=h2;
                h3=h2;
                h2=t.next;
            }


            while(h1 != null && h2 != null){
                if(h1.val < h2.val){
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
    
}
```

### BETTER CODE

```java
class Solution {
   
    public ListNode mergeKLists(ListNode[] lists) {
        return mergeKListsHelper(lists, 0, lists.length-1);
    }

    public ListNode mergeKListsHelper(ListNode[] lists, int i, int j) {

        if(j<i) return null;
        if(i==j) return lists[i];

        int m = (i+j)/2;

        ListNode h1 = mergeKListsHelper(lists, i, m);
        ListNode h2 = mergeKListsHelper(lists, m+1, j);
        
        return mergeSortedList(h1, h2);

    }

    public ListNode mergeSortedList(ListNode list1, ListNode list2){

        if(list1 == null && list2 == null) return null;
        if(list1 == null) return list2;
        if(list2 == null) return list1;

        ListNode h3 = null;

        if(list1.val <= list2.val){
            h3 = list1;
            list1.next = mergeSortedList(list1.next, list2);
        }else{
            h3 = list2;
            list2.next = mergeSortedList(list1, list2.next);
        }

        return h3;

    }

}
```
