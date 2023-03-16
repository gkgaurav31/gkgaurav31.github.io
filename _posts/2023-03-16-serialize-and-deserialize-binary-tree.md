---
layout: post
title: Serialize and Deserialize Binary Tree
date: 2023-03-16 19:27 +0530
tags: "java binarytree neetcode150 important"
categories: "neetcode150"
---

### PROBLEM DESCRIPTION

Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.

Clarification: The input/output format is the same as how LeetCode serializes a binary tree. You do not necessarily need to follow this format, so please be creative and come up with different approaches yourself.

[leetcode](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/description/)

### SOLUTION

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Codec {

    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        
        //Example: [1, 2, N, N, 3, 4, N, N, 5, N, N] -> preorder
        //N -> null
        List<String> list = serializeHelper(root, new ArrayList<>());

        //Convert the above list of strings to a single string separated by comma
        return covertListToStringWithDelimiter(list);

    }

    //convert list of string to a single string delimited by comma
    public String covertListToStringWithDelimiter(List<String> list){

        StringBuffer sb = new StringBuffer();
        
        //if the list does not have any string, return empty string
        if(list.size() == 0) return "";

        //add the first string
        sb.append(list.get(0));

        //for subsequent string add a comma and then the next string
        for(int i=1; i<list.size(); i++){
            sb.append("," + list.get(i));
        }

        return sb.toString();
    }

    //pre-order traversal
    public List<String> serializeHelper(TreeNode root, List<String> list) {

        if(root == null){
            list.add("N");
            return list;
        }

        list.add(String.valueOf(root.val));
        serializeHelper(root.left, list);
        serializeHelper(root.right, list);

        return list;

    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {

        //split the string by "," to get the array of strings
        String[] arr = data.split(",");

        //covert to list to make it easy to remove the first element
        List<String> data_list = new LinkedList<String>(Arrays.asList(arr));

        return deserializeHelper(data_list);
    }


    public TreeNode deserializeHelper(List<String> list) {

        //if the first element is N, that means it's null, we can move forward since default value will be null for next and right 
        if(list.get(0).equals("N")){
            list.remove(0);
            return null;
        }

        //if it's not null, create a node using the value as int
        int val = Integer.parseInt(list.get(0));
        TreeNode node = new TreeNode(val);

        //remove it from the list
        list.remove(0);

        //recursively set its left pointer and right pointers
        //since we know that the list "pre-ordered", the immediate next will be on the left of it
        //it will keep getting removed from the list until we get a null (base case)
        //at that point, the node.right will get called
        node.left = deserializeHelper(list);
        node.right = deserializeHelper(list);

        return node;

    }

    

}

// Your Codec object will be instantiated and called as such:
// Codec ser = new Codec();
// Codec deser = new Codec();
// TreeNode ans = deser.deserialize(ser.serialize(root));
```
