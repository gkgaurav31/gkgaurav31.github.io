---
layout: post
title: Populating Next Right Pointers in Each Node II
date: 2023-08-11 20:02 +0530
author: "Gaurav Kumar"
tags: "java binarytree leetcode leetcode150 important"
categories: "binarytree"
---
## PROBLEM DESCRIPTION

Given a binary tree

```text
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```

Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.

Initially, all next pointers are set to NULL.

[leetcode](https://leetcode.com/problems/populating-next-right-pointers-in-each-node-ii/)

## SOLUTION

### APPROACH 1 (EXTRA SPACE)

The idea is to perform a level order traversal of the binary tree (skip the null nodes). Then iterate through nodes at each level and update their next pointers.

```java
class Solution {
    
    public Node connect(Node root) {

        // If the root is null, there's nothing to connect, so return null.
        if (root == null) return null;

        // Create a queue to perform level-order traversal.
        Queue<Node> q = new LinkedList<>();

        // Create a list to store nodes at each level.
        List<List<Node>> list = new ArrayList<>();

        // Add the root node to the queue to start traversal.
        q.add(root);
        // Add a marker (null) to indicate the end of the current level.
        q.add(null);

        // Continue traversal until there's only the null marker left in the queue.
        while (q.size() > 1) {

            // Create a list to store nodes at the current level.
            List<Node> currentList = new ArrayList<>();

            // Process nodes at the current level until the null marker is encountered.
            while (q.peek() != null) {

                // Remove a node from the queue.
                Node node = q.poll();

                // Add the node to the list of the current level.
                currentList.add(node);

                // Add the left and right child nodes of the current node to the queue for the next level.
                if (node.left != null) q.add(node.left);
                if (node.right != null) q.add(node.right);
            }

            // Remove the null marker from the queue to move to the next level.
            // Add a new null marker to the queue for the next level.
            q.poll(); q.add(null);

            // If there are nodes at the current level, add the list to the overall list of levels.
            if (currentList.size() > 0) list.add(currentList);
        }

        // Traverse through the list of levels.
        for (int i = 0; i < list.size(); i++) {

            // Get the current node
            List<Node> l = list.get(i);

            // Traverse from first to last-but-one node because we do not need to update the next pointer for the last node
            // Update the next pointer of the current node to point to next node in that level
            for (int j = 0; j < l.size() - 1; j++) {
                l.get(j).next = l.get(j + 1);
            }
        }

        // Return the modified root of the binary tree.
        return root;
    }
}
```

### APPROACH 2 (CONSTANT SPACE)

The idea is to start with the leftmost node for each level. For the first level, we obviously start with the root node. An important thing to observe in this algorithm is that we establish the next pointers for a level N while we are still on level N−1 and once we are done establishing these new connections, we move on to N and do the same thing for N+1.  

It would go something like this:

- Start at the root node (initially, the leftmost node at level 0).
- Reset the `leftmost` reference to null; it will be updated to the first non-null node in the next level.
- For each node at the current level:
  - Check its left and right children.
  - If the left child is not null:
    - If there's no previous node (`prev` is empty), this is the first node of the next level, so update `leftmost`.
    - Otherwise, connect the previous node's `next` pointer to the current left child.
  - If the right child is not null:
    - If there's no previous node (`prev` is empty), this is again the first node of the next level, so update `leftmost`.
    - Otherwise, connect the previous node's `next` pointer to the current right child.
- Once this level is processed, the `next` pointers for the next level are set. Reset to `leftmost` and repeat the process.
- If there are no nodes at the next level, `leftmost` remains null, and the loop terminates.

```java
class Solution {
    
    public Node connect(Node root) {

        // Start with the leftmost node of the current level.
        Node leftmost = root;

        // Traverse each level of the tree.
        while (leftmost != null) {

            // Initialize variables to traverse the current level. (Like a LinkedList)
            Node current = leftmost; // The current node being processed in the current level.
            Node prev = null; // The previous node that has been processed and connected. This will be used to update the next pointer of previous node to the current node
            
            // Reset leftmost for the next level.
            // If leftmost is still null after the next while loop, it means that there are no nodes in the next level, so we can end
            // We will need the starting point for the next level also. So, as soon as we get the first non-null node, we update leftmost
            leftmost = null; 

            // Traverse the current level.
            while (current != null) {

                Node leftChild = current.left; // Left child of the current node.
                Node rightChild = current.right; // Right child of the current node.

                // If left child is not null
                if (leftChild != null) {

                    // If prev is null, it means that we have not got any non-null nodes yet in this level
                    // This will be the leftmost node for the next level, so update it
                    if (prev == null) {
                        leftmost = leftChild; 
                    // Otherwise, we would have got some node before this for which we need to update the next pointer to the current child
                    } else {
                        prev.next = leftChild; // Connect the previous node's next to the left child.
                    }

                    prev = leftChild; // Update prev for the next iteration.
                }

                // Similarly for the right child as well
                if (rightChild != null) {
                    if (prev == null) {
                        leftmost = rightChild; // Update leftmost if it's the first node in the level.
                    } else {
                        prev.next = rightChild; // Connect the previous node's next to the right child.
                    }
                    prev = rightChild; // Update prev for the next iteration.
                }

                current = current.next; // Move to the next node in the current level.
            }
        }

        return root; // Return the modified root of the binary tree.
    }
}
```
