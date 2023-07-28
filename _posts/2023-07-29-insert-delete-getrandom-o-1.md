---
layout: post
title: Insert Delete GetRandom O(1)
date: 2023-07-29 02:26 +0530
author: "Gaurav Kumar"
tags: "java arrays leetcode leetcode150"
categories: "arrays"
---

## PROBLEM DESCRIPTION

Implement the RandomizedSet class:

- RandomizedSet() Initializes the RandomizedSet object.
- bool insert(int val) Inserts an item val into the set if not present. Returns true if the item was not present, false otherwise.
- bool remove(int val) Removes an item val from the set if present. Returns true if the item was present, false otherwise.
- int getRandom() Returns a random element from the current set of elements (it's guaranteed that at least one element exists when this method is called). Each element must have the same probability of being returned.
- You must implement the functions of the class such that each function works in average O(1) time complexity.

[leetcode](https://leetcode.com/problems/insert-delete-getrandom-o1/)

## SOLUTION

If we use only HashMap or HashSet, we can get and remove items in constant time, however, the random function will have to be linear time because items in a set are not in any order. Array/List can help with random function in constant time, however, removal of items is linear for most cases. But, if we have to remove only the last element, it's O(1).

- We can create a List/Array to add items. A HashMap which will check if an item is present and return its index in O(1)
- When we want to remove an element, since we know the index, we can first override it with the last element and reduce the list size simply! (we will need to update the HashMap to ensure that it stores the updated index)

```java
class RandomizedSet {

    Map<Integer, Integer> map; //<Element, IndexInTheList>
    List<Integer> list; //list of elements added

    public RandomizedSet() {
        map = new HashMap<Integer, Integer>();
        list = new ArrayList<>();
    }
    
    public boolean insert(int val) {
        
        //check if already present
        if(map.containsKey(val)) return false;

        list.add(val);
        map.put(val, list.size()-1);

        return true;

    }
    
    public boolean remove(int val) {
        
        //check if not present
        if(!map.containsKey(val)) return false;

        //get the last element
        int lastElementVal = list.get(list.size()-1);

        //get index of val
        int idx = map.get(val);

        //override list[idx] with the last element
        list.set(idx, lastElementVal);
        //update new index
        map.put(lastElementVal, idx);

        //remove last element
        list.remove(list.size()-1);
        map.remove(val);

        return true;

    }
    
    public int getRandom() {
        return list.get(new Random().nextInt(list.size()));
    }
}

/**
 * Your RandomizedSet object will be instantiated and called as such:
 * RandomizedSet obj = new RandomizedSet();
 * boolean param_1 = obj.insert(val);
 * boolean param_2 = obj.remove(val);
 *85 int param_3 = obj.getRandom();
 */
```
