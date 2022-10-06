---
layout: post
title: Right Smaller Than
date: 2022-10-06 23:00 +0530
author: "Gaurav Kumar"
tags: "java BST important"
categories: "BST"
---

### PROBLEM DESCRIPTION

Write a function that takes in an array of integers and returns an array of the same length where each element in the output array corresponds to the number of integers in the input array that are to the right of the relevant index and that are strictly smaller than the integer at that index.  

### SOLUTION

![snapshot]({{ site.baseurl }}/assets/img/right_smaller_than.png)

```java
import java.util.*;

class Program {

  static List<Integer> ans = new ArrayList<>();
  
  static class BSTNode{
    
    int val;
    int LST;
    BSTNode left;
    BSTNode right;
   
    BSTNode(int val){
      this.val = val;
      LST = 0;
      left = null;
      right = null;
    }

    BSTNode insert(BSTNode root, int k, int[] count){
      
      if(root == null){
         root = new BSTNode(k);
        return root;
      }

      if(k < root.val){
        root.LST++;
        root.left = insert(root.left, k, count);
      }else{
        if(k > root.val){
          count[0]++;
        }
        
        count[0] += root.LST;
        
        root.right = insert(root.right, k, count);
      }

      return root;
    }
    
  }
  
  public static List<Integer> rightSmallerThan(List<Integer> array) {

    if(array.size() == 0) return new ArrayList<>();
    if(array.size() == 1) return Arrays.asList(new Integer[]{0});

    ans = new ArrayList<>();
    
    BSTNode root = new BSTNode(array.get(array.size()-1));
    ans.add(0);

    for(int i=array.size()-2; i>=0; i--){
      int k = array.get(i);
      int[] c = new int[1];
      root.insert(root, k, c);
      ans.add(c[0]);
    }
    Collections.reverse(ans);
    return ans;
  }
}
```
