---
layout: post
title: All Subsets II
date: 2022-10-25 21:50 +0530
author: "Gaurav Kumar"
tags: "java recursion backtracking"
categories: "backtracking"
---

## Problem Description

Given a set of distinct integers A, return all possible subsets.

__NOTE:__

- Elements in a subset must be in non-descending order.
- The solution set must not contain duplicate subsets.
- Also, the subsets should be sorted in ascending ( lexicographic ) order.
- The list is not necessarily sorted.

### Solution

For each element, we can two choices - either to include that element or exclude it from the current subset.

```java
public class Solution {

    ArrayList<ArrayList<Integer>> ans = new ArrayList<>();

    public ArrayList<ArrayList<Integer>> subsets(ArrayList<Integer> A) {
        
        //We can sort the list since the problem needs the elements in sorted order in each subset
        Collections.sort(A);
        
        //main helper method that will generate all subsets and add to answer list
        helper(A, new ArrayList<>(), 0);

        //The subsets themselves need to be sorted for which we will need to use a comparator
        Collections.sort(ans, new Comparator<ArrayList<Integer>>(){

            @Override
            public int compare(ArrayList<Integer> o1, ArrayList<Integer> o2) {

                //Loop through each element in first list and check if any of the element is smaller/larger
                for(int i=0; i<o1.size() && i<o2.size(); i++){
                    if(o1.get(i) > o2.get(i)) return 1;
                    if(o1.get(i) < o2.get(i)) return -1;
                }

                //If all elements in list1 are same as list2, we need to compare their size
                if(o1.size() > o2.size()) return 1;
                else return -1;

            }

        });

        return ans;

    }

    public void helper(ArrayList<Integer> list, ArrayList<Integer> current, int i){

        //When i is equal to size of the list, we must have taken a decision whether to include or exclude each of the elements
        //At this point, we can add the subset we have generated
        if(i == list.size()){
            //Note that we cannot simply do: ans.add(current)
            //The reference would be the same, so the same list would get modified
            ans.add(new ArrayList<>(current));
            return;
        }

        //Including the current element
        current.add(list.get(i));
        helper(list, current, i+1);

        //Excluding the current element
        //We need to remove from current list because we had added it in the previous step
        current.remove(current.size()-1);
        helper(list, current, i+1);

    }
}
```

### ANOTHER APPROACH (Not optimized)

```java
public class Solution {

    public int[][] subsets(int[] A) {

        int n = A.length;

        //total number of subsets will be 2^n
        int totalSubsets = (1<<n);

        //sort the array so that we form subsets which are already sorted (individual subsets will be sorted only)
        Arrays.sort(A);

        //answer list
        ArrayList<ArrayList<Integer>> list = new ArrayList<>();

        //basically we take taking numbers from [0, 2^n]
        //each one will have a different bit set/unset and they will also form different subsets
        //if kth bit is set in the current number, then include A[k] in the current subset
        //Example: i = 0101 => current subset will be: {A[0], A[2]} because the 0th and 2nd bit are set. 
        for(int i=0; i<totalSubsets; i++){

            ArrayList<Integer> subset = new ArrayList<>();

            for(int pos=0; pos<n ; pos++){

                if(checkSetBit(pos, i)){
                    subset.add(A[pos]);
                }

            }

            list.add(subset);

        }

        //sort the complete arraylist
        list = sortList(list);

        return convertListTo2DArray(list);

    }

    public ArrayList<ArrayList<Integer>> sortList(ArrayList<ArrayList<Integer>> list) {
        Collections.sort(list, new Comparator<ArrayList<Integer>>() {
            @Override
            public int compare(ArrayList<Integer> o1, ArrayList<Integer> o2) {
                int n1 = o1.size(), n2 = o2.size();
                for (int i = 0; i < Math.min(n1, n2); i++) {
                    int cmp = o1.get(i).compareTo(o2.get(i));
                    if (cmp != 0) {
                        return cmp;
                    }
                }
                return Integer.compare(n1, n2);
            }
        });
        return list;
    }

    public boolean checkSetBit(int pos, int n){

        if( ((1<<pos)&n) != 0)
            return true;
        else
            return false;

    }

    public static int[][] convertListTo2DArray(ArrayList<ArrayList<Integer>> list) {
        int[][] arr = new int[list.size()][];
        for (int i = 0; i < list.size(); i++) {
            ArrayList<Integer> sublist = list.get(i);
            arr[i] = new int[sublist.size()];
            for (int j = 0; j < sublist.size(); j++) {
                arr[i][j] = sublist.get(j);
            }
        }
        return arr;
    }


}
```
