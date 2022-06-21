---
layout: post
title: Min Height BST
date: 2022-06-21 21:27 +0530
author: "Gaurav Kumar"
tags: "java BST"
categories: "algoexpert"
---

### PROBLEM DESCRIPTION

Given a list of integers in sorted order, return the root node of a BST such that the BST has the minimum possible height.

### SOLUTION

Since the list of already sorted, we can insert the mid of the list first. The elements to the left will be smaller than the mid element, so they will all go to the left sub tree. Similar for the RST. We can do this recursively by calling the method and passing the sublist.

:exclamation: In this approach, we have used the subList method which has just O(1) time complexity. See: [List.subList](https://docs.oracle.com/javase/8/docs/api/java/util/List.html#subList-int-int-)

```java
import java.util.*;

class Program {
  public static BST minHeightBst(List<Integer> array) {
    if(array.size() == 0)
      return null;
    int mid = array.size()/2;
    BST root = new BST(array.get(mid));
    root.left = minHeightBst(array.subList(0,mid));
    root.right = minHeightBst(array.subList(mid+1, array.size()));
    return root;
  }

  static class BST {
    public int value;
    public BST left;
    public BST right;

    public BST(int value) {
      this.value = value;
      left = null;
      right = null;
    }

    public void insert(int value) {
      if (value < this.value) {
        if (left == null) {
          left = new BST(value);
        } else {
          left.insert(value);
        }
      } else {
        if (right == null) {
          right = new BST(value);
        } else {
          right.insert(value);
        }
      }
    }
  }
}
```

Thanks @[neha502](https://github.com/neha502)
