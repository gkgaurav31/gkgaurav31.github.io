---
layout: post
title: Clone Linked List with Random Pointer
date: 2022-09-11 00:44 +0530
author: "Gaurav Kumar"
tags: "java linkedlist leetcode important"
---

### PROBLEM DESCRIPTION

A linked list of length n is given such that each node contains an additional random pointer, which could point to any node in the list, or null. Construct a deep copy of the list.

```java
class Node {
    int val;
    Node next;
    Node random;

    public Node(int val) {
        this.val = val;
        this.next = null;
        this.random = null;
    }
}
```

### SOLUTION

![snapshot]({{ site.baseurl }}/assets/img/clone_list_with_random_pointer.png)

```java
class Solution {

    //Create a clone without considering random pointer
    public Node cloneLinkedList(Node h1){

        if(h1 == null) return null;

        Node h2 = new Node(h1.val);

        Node t1 = h1.next;
        Node t2 = h2;

        while(t1 != null){
            t2.next = new Node(t1.val);
            t1 = t1.next;
            t2 = t2.next;
        }

        return h2;

    }

    //Return a HashMap which has a mapping between old and new nodes which are clones of each other
    public HashMap<Node,Node> getHashMap(Node h1, Node h2){

        HashMap<Node, Node> set = new HashMap<>();

        Node t1 = h1;
        Node t2 = h2;

        while(t1 != null && t2 != null){
            set.put(t1, t2);
            t1 = t1.next;
            t2 = t2.next;
        }

        return set;

    }

    public Node copyRandomList(Node head) {

        Node h2 = cloneLinkedList(head);

        HashMap<Node, Node> set = getHashMap(head, h2);

        Node t1 = head;
        Node t2 = h2;

        while(t1 != null && t2 != null){    //We can check for any one of them to be null, that should also work since the two lists have same length

            Node r1 = t1.random;    //Random Node in the original list
            Node r2 = set.get(r1);  //Corresponding Node in the cloned list

            t2.random = r2;         //Update the current Node in clone list to the required random Node present in the clone list

            //Move to next node
            t1 = t1.next;
            t2 = t2.next;

        }

        return h2;

    }
}
```

### ANOTHER WAY TO CODE

```java
class Solution {

    public Node copyRandomList(Node head) {

        // HashMap: <OldNode, NewNOde>
        Map<Node, Node> map = new HashMap<>();

        Node h1 = head;

        // iterate through all the old nodes
        // create copied of it based on its value
        // add them to the hashmap
        // at this point, we are ignoring the random pointer
        while(h1 != null){
            Node t = new Node(h1.val);
            map.put(h1, t);
            h1 = h1.next;
        }

        // reset h1 to start of the linked list
        h1 = head;

        // iterate through all the nodes
        while(h1 != null){

            // actual next of the current node
            Node originalNext = h1.next;

            // actual random of the current node
            Node originalRandom = h1.random;

            // update next of cloned node with the corresponding node using HashMap
            map.get(h1).next = map.get(originalNext);

            // update random of current node with the corresponding node using HashMap
            map.get(h1).random = map.get(h1.random);

            // move to the next node
            h1 = h1.next;

        }

        // the new linked list will start with the cloned head
        // we can get it from the HashMap
        return map.get(head);

    }

}
```
