---
layout: post
title: Amount of Time for Binary Tree to Be Infected
date: 2025-01-31 23:03 +0530
author: "Gaurav Kumar"
tags: "java leetcode binarytree important"
categories: "binarytree"
---

## PROBLEM DESCRIPTION

You are given the root of a binary tree with unique values, and an integer start. At minute 0, an infection starts from the node with value start.

Each minute, a node becomes infected if:

The node is currently uninfected.
The node is adjacent to an infected node.
Return the number of minutes needed for the entire tree to be infected.

[leetcode](https://leetcode.com/problems/amount-of-time-for-binary-tree-to-be-infected/description/)

## SOLUTION

### APPROACH 1

If this were an **undirected graph**, we could simply use **BFS** starting from the source node to determine the number of levels. The number of levels would directly correspond to the time required to infect all nodes.

However, in the case of a **binary tree**, we lack direct pointers to parent nodes. To overcome this, we can first create a mapping of each node to its parent without modifying the tree structure. We can achieve this using a **HashMap**, where the `key` is the `node value` and the `value` is a reference to its `parent node`:

`HashMap<Node Value, Parent Node Reference>`

The problem states that all nodes are unique.

#### BFS Traversal

Once we have the parent mapping, we can perform a **BFS traversal** starting from the given `start` node:

1. Add the `start` node to the queue and mark it as visited.
2. For each node, check its **left** and **right** children—if they haven't been visited, add them to the queue and mark them as visited.
3. Similarly, retrieve the **parent node** from the HashMap and add it to the queue if it hasn't been visited.

### Edge Case

- The root node has no parent, so its parent reference in the HashMap will be `null`. We need to handle this case separately.

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {

    // the node which has the value of "start" (all nodes have unique value in this problem)
    TreeNode sourceNode = null;

    public int amountOfTime(TreeNode root, int start) {

        // create a HashMap to determine the parent for any given node
        // Map -> [SourceNode Value, ParentNode]
        Map<Integer, TreeNode> map = createParentMapping(root, start);

        // use BFS to determine the number of level (which will equal to the time needed), from source node
        return burnTimeFromSource(root, map);

    }

    // use BFS from source node
    // binary tree don't have a parent poiner. we will use the map[child, parent] mapping for this
    public int burnTimeFromSource(TreeNode root, Map<Integer, TreeNode> map){

        // to avoid visisted the same node again
        Set<Integer> visited = new HashSet<>();

        // queue for bfs
        Queue<TreeNode> q = new LinkedList<>();

        // add the source node and mark as visited
        q.add(sourceNode);
        visited.add(sourceNode.val);

        // init time:
        // for the first level, the time should be 0
        int time = -1;

        // keep traversing level by level while queue is not empty
        while(!q.isEmpty()){

            // we are at the next level, so increase time by 1
            time++;

            // current number of elements in this level
            int size = q.size();

            // explore the nodes at current level
            for(int i=0; i<size; i++){

                // current node in this level
                TreeNode current = q.poll();

                // if it has a left node, and it has not been visited yet
                // add it to the queue and mark it as visited
                if(current.left != null && !visited.contains(current.left.val)){
                    q.add(current.left);
                    visited.add(current.left.val);
                }

                // do the same for its right node
                if(current.right != null && !visited.contains(current.right.val)){
                    q.add(current.right);
                    visited.add(current.right.val);
                }

                // check if it has any parent in hashmap
                // important: ensure it is not null -> this will happen for root node (edge case)
                // ensure it has not been visited before
                // add it to the queue and mark it as visited
                if(map.containsKey(current.val) && map.get(current.val) != null && !visited.contains(map.get(current.val).val)){
                    q.add(map.get(current.val));
                    visited.add(map.get(current.val).val);
                }

            }

        }

        return time;

    }

    // Get Map -> [Source Node Value, Parent Node Reference]
    public Map<Integer, TreeNode> createParentMapping(TreeNode root, int start){

        Map<Integer, TreeNode> map = new HashMap<>();

        // Queue for level order traversal
        Queue<TreeNode> q = new LinkedList<>();
        q.add(root);

        // We need to add root node also, whose parent is null
        // We will need to handle this null during BFS later
        map.put(root.val, null);

        // while queue is not empty
        while(!q.isEmpty()){

            // find the size of queue
            int size = q.size();

            // take out the elements from the queue based on the size calculated earlier
            for(int i=0; i<size; i++){

                // current node taken out
                TreeNode current = q.poll();

                // if it is equal to "start", save it (this will be used later while calculating time)
                if(current.val == start)
                    sourceNode = current;

                // if left node is present, add it to the queue to explore later
                // also put the mapping for its child node in the hashmap
                if(current.left != null){
                    map.put(current.left.val, current);
                    q.add(current.left);
                }

                // do the same for right node
                if(current.right != null){
                    map.put(current.right.val, current);
                    q.add(current.right);
                }

            }

        }

        return map;

    }


}
```
