---
layout: post
title: LRU Cache (geeksforgeeks - SDE Sheet)
date: 2024-10-20 19:31 +0530
author: "Gaurav Kumar"
tags: "java design linkedlist geeksforgeeks sde-sheet important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Design a data structure that works like a LRU Cache. Here cap denotes the capacity of the cache and Q denotes the number of queries. Query can be of two types:

- SET x y: sets the value of the key x with value y
- GET x: gets the key of x if present else returns -1.

The LRUCache class has two methods get() and set() which are defined as follows.

- get(key): returns the value of the key if it already exists in the cache otherwise returns -1.
- set(key, value): if the key is already present, update its value. If not present, add the key-value pair to the cache. If the cache reaches its capacity it should remove the least recently used item before inserting the new item.
  In the constructor of the class the capacity of the cache should be initialized.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/lru-cache/1?page=3)

## SOLUTION

Refer to the following blog for initial approaches:
[LRU Cache Approach]({% post_url 2023-03-07-lru-cache %})

```java
class LRUCache {

    int capacity; // Maximum capacity of the cache
    Node head;
    Node tail;

    // HashMap to store key and corresponding node reference
    Map<Integer, Node> map;

    int size;     // Current size of the cache

    LRUCache(int cap) {

        this.capacity = cap;
        map = new HashMap<>();
        size = 0;

        // Create dummy head and tail nodes for easier manipulation of the doubly linked list
        head = new Node(-1, -1);
        tail = new Node(-1, -1);

        // Initially, the doubly linked list is empty, so head connects to tail
        head.next = tail;
        tail.prev = head;

    }

    // Function to return value corresponding to the key.
    public int get(int key) {

        // If the key is not present in the cache, return -1
        if (!map.containsKey(key))
            return -1;

        // Get the corresponding node from the map
        Node node = map.get(key);

        // Move the accessed node to the end of the doubly linked list (mark it as recently used)
        // First, remove it from its current position
        Node prevNode = node.prev;
        Node nextNode = node.next;
        prevNode.next = nextNode;
        nextNode.prev = prevNode;

        // Then, add it to the end (before the tail)
        Node lastNode = tail.prev;
        lastNode.next = node;
        node.prev = lastNode;
        node.next = tail;
        tail.prev = node;

        // Return the value associated with the key
        return node.value;
    }

    // Function for storing key-value pair.
    public void set(int key, int value) {

        // First, check if the key is already present using the get() method
        int x = get(key);

        // If the key is already present, update its value (it has already been moved to the end by get())
        if (x != -1) {
            tail.prev.value = value;
            return;
        }

        // If the key is not present, create a new node for this key-value pair
        Node node = new Node(key, value);
        map.put(key, node);

        // Check if the cache has reached its capacity
        if (size >= capacity) {
            // Remove the least recently used node (the node right after the dummy head)
            Node removeNode = head.next;

            // Remove it from the map using its key
            map.remove(removeNode.key);

            // Remove it from the doubly linked list
            Node newNext = head.next.next;
            head.next = newNext;
            newNext.prev = head;

        } else {

            // If cache is not full, increment the size
            size++;

        }

        // Add the new node to the end of the doubly linked list (mark it as recently used)
        Node lastNode = tail.prev;
        lastNode.next = node;
        node.prev = lastNode;
        node.next = tail;
        tail.prev = node;

    }
}

// Node for a Doubly Linked List
class Node {

    int key;
    int value;
    Node prev;
    Node next;

    Node(int k, int v) {
        key = k;
        value = v;
    }
}
```

## ORGANIZED CODE

Some parts of the previous code can be separately put in a different method for better readability. The logic remains the same.

```java
class LRUCache {

    int capacity;
    Node head;
    Node tail;
    Map<Integer, Node> map;
    int size;

    // Constructor for initializing the cache capacity with the given value.
    LRUCache(int cap) {

        this.capacity = cap;
        map = new HashMap<>();
        size = 0;

        // dummy
        head = new Node(-1, -1);
        tail = new Node(-1, -1);

        head.next = tail;
        tail.prev = head;

    }

    // Function to return value corresponding to the key.
    public int get(int key) {

        if(!map.containsKey(key))
            return -1;

        // get the corresponding node
        Node node = map.get(key);

        // move it to the end
        deleteNode(node);
        addToEnd(node);

        return node.value;

    }

    // Function for storing key-value pair.
    public void set(int key, int value) {

        int x = get(key);

        if(x != -1){
            tail.prev.value = value;
            return;
        }

        Node node = new Node(key, value);
        map.put(key, node);

        if(size >= capacity){

            // remove oldest node
            deleteFront();

        }else
            size++;

        // add current node to the end
        addToEnd(node);

    }

    public void deleteFront(){

        Node removeNode = head.next;
        map.remove(removeNode.key);

        // remove from linked list
        Node newNext = head.next.next;

        head.next = newNext;
        newNext.prev = head;

    }

    public void deleteNode(Node node){

        Node prevNode = node.prev;
        Node nextNode = node.next;
        prevNode.next = nextNode;
        nextNode.prev = prevNode;

    }

    public void addToEnd(Node node){

        Node lastNode = tail.prev;
        lastNode.next = node;
        node.prev = lastNode;
        node.next = tail;
        tail.prev = node;

    }

}

// Node for a Doubly Linked List
class Node{

    int key;
    int value;
    Node prev;
    Node next;

    Node(int k, int v){
        key = k;
        value = v;
    }

}
```
