---
layout: post
title: Serialize and deserialize a binary tree (geeksforgeeks - SDE Sheet)
date: 2024-09-20 18:42 +0530
author: "Gaurav Kumar"
tags: "java binarytree geeksforgeeks sde-sheet important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Serialization is to store a tree in an array so that it can be later restored and deserialization is reading tree back from the array. Complete the functions

- serialize() : stores the tree into an array a and returns the array.
- deSerialize() : deserializes the array to the tree and returns the root of the tree.

Note: Multiple nodes can have the same data and the node values are always positive integers. Your code will be correct if the tree returned by deSerialize(serialize(input_tree)) is same as the input tree. Driver code will print the in-order traversal of the tree returned by deSerialize(serialize(input_tree))

[geeksforgeeks](https://www.geeksforgeeks.org/problems/serialize-and-deserialize-a-binary-tree/1?page=8)

## SOLUTION

```java
class Tree {
    // Function to serialize a tree and return a list containing nodes of tree.
    public ArrayList<Integer> serialize(Node root) {
        return serializeHelper(root, new ArrayList<>());
    }

    public ArrayList<Integer> serializeHelper(Node root, ArrayList<Integer> preorder){

        if(root == null){
            preorder.add(-1);
            return preorder;
        }

        preorder.add(root.data);

        serializeHelper(root.left, preorder);
        serializeHelper(root.right, preorder);

        return preorder;

    }

    // Function to deserialize a list and construct the tree.
    public Node deSerialize(ArrayList<Integer> A) {

        if(A.get(0) == -1){
            A.remove(0);
            return null;
        }

        Node node = new Node(A.get(0));
        A.remove(0);

        node.left = deSerialize(A);
        node.right = deSerialize(A);

        return node;

    }
};
```

## SMALL OPTIMIZATION

We can avoid removing elements from the list during deserialization by using an index variable.

```java
class Tree {
    // Function to serialize a tree and return a list containing nodes of tree.
    public ArrayList<Integer> serialize(Node root) {
        return serializeHelper(root, new ArrayList<>());
    }

    public ArrayList<Integer> serializeHelper(Node root, ArrayList<Integer> preorder){

        if(root == null){
            preorder.add(-1);
            return preorder;
        }

        preorder.add(root.data);

        serializeHelper(root.left, preorder);
        serializeHelper(root.right, preorder);

        return preorder;

    }

    // Function to deserialize a list and construct the tree.
    public Node deSerialize(ArrayList<Integer> A) {
        return deSerializeHelper(A, new int[]{0});
    }

    public Node deSerializeHelper(ArrayList<Integer> A, int[] idx){

        if(idx[0] >= A.size() || A.get(idx[0]) == -1){
            idx[0]++;
            return null;
        }

        Node node = new Node(A.get(idx[0]));

        idx[0]++;

        node.left = deSerializeHelper(A, idx);
        node.right = deSerializeHelper(A, idx);

        return node;

    }

};
```
