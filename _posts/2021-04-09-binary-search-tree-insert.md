---
layout: post
title: Binary Search Tree - Insert
date: 2021-04-09 00:13 +0530
author: "Gaurav Kumar"
tags: "java"
categories: "geeksforgeeks"
---

### PROBLEM DESCRIPTION: [https://practice.geeksforgeeks.org/problems/insert-a-node-in-a-bst/1#](https://practice.geeksforgeeks.org/problems/insert-a-node-in-a-bst/1#)

```PLAINTEXT
Given a BST and a key K. If K is not present in the BST, Insert a new Node with a value equal to K into the BST. 
Note: If K is already present in the BST, don't modify the BST.
```

### SOLUTION

```java

//User function Template for Java

// class Node  
// { 
//     int data; 
//     Node left, right; 
   
//     public Node(int d)  
//     { 
//         data = d; 
//         left = right = null; 
//     } 
// }

class Solution{
    
    // The function returns the root of the BST (currently rooted at 'root') 
    // after inserting a new Node with value 'Key' into it. 
    Node insert(Node root, int Key)
    {
      
        //Create temporary node for traversal
        Node current = root;
        
        //While the current node is not null:
        while(current != null){

        //If the current node has the key, end
        if(current.data == Key)
        break;
         
        //If not, check if the Key is less than the data in current node. Move to left if that's the case
        else if(Key < current.data){
            //If left Node of current is not null, go to left Node since we can find for Key on the left 
            if(current.left != null)
                current = current.left;
            //If left Node is null, since Key is less than the current node.data, we will need to add a Node to left
            else
            {
                Node newNode = new Node(Key);
                current.left = newNode;
                break;
            }
         }
        //Same logic but on the right part of the tree
        else{
            if(current.right != null)
            current = current.right;
            else{
                Node newNode = new Node(Key);
                current.right = newNode;
                break;
            }
        }
        }
        
        //return root as per the question
        return root;
    }
}
```
