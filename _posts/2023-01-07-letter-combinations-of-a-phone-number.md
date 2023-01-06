---
layout: post
title: Letter Combinations of a Phone Number
date: 2023-01-07 00:14 +0530
author: "Gaurav Kumar"
tags: "java recursion backtracking"
categories: "backtracking"
---

## Problem Description

Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent. Return the answer in any order.

A mapping of digits to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

[leetcode](https://leetcode.com/problems/letter-combinations-of-a-phone-number/description/)

![snapshot]({{ site.baseurl }}/assets/img/telephone-chars.png)

### Solution

We populate an Array of List of Characters in which every digit points to a List<Character> which are possible if that button was pressed. For each digit in the input, we loop through every possible character which are possible. Then, we recursively call the method for the next digit. We do this until the number of characters we have formed are equal to the size of the input digits.  

Instead of using array of list of characters, we can also use a HashMap:

```java
 Map<Character, String> letters = Map.of(
        '2', "abc", '3', "def", '4', "ghi", '5', "jkl", 
        '6', "mno", '7', "pqrs", '8', "tuv", '9', "wxyz");
```

```java
class Solution {
    
    public List<String> letterCombinations(String digits) {
        
        //edge case
        if(digits.equals("")) return new ArrayList<String>();

        //convert dial pad as an Array of List of characters. 
        //index 0, index 1 -> null
        //index 2 -> [a, b, c]
        //index 3 -> [b, c, d]
        //...
        //index 8 -> [t u v]
        //index 9 -> [w, x, y, z]
        List<Character>[] nums = createList();

        combinationHelper(digits, nums, "", 0);
        return ans;
        
    }

    List<String> ans = new ArrayList<>();

    public void combinationHelper(String digits, List<Character>[] nums, String current, int idx){
        
        //idx will be tracking the number of digits/characters considered already
        //if it is equal to the number of digits in the input, we can include that as one of the combinations
        if(idx >= digits.length()){
            ans.add(current);
            return;
        }

        //get current digit in the input
        int c = Integer.parseInt(digits.charAt(idx)+"");

        //get the list of characters it can denote
        List<Character> characters = nums[c];

        //loop through each possible character
        for(int i=0; i<characters.size(); i++){
            
            //append that in the current string
            current += characters.get(i);

            //recursively call combinationHelper, going to the next digit
            combinationHelper(digits, nums, current, idx+1);

            //backtrack -> restore the current string by removing the last character which was appended
            current = current.substring(0, current.length()-1);

        }

    }


    public List[] createList(){

        List<Character>[] nums = new List[10];

        List<Character> temp = new ArrayList<>();
        temp.add('a');
        temp.add('b');
        temp.add('c');
        
        nums[2] = temp;

        temp = new ArrayList<>();
        temp.add('d');
        temp.add('e');
        temp.add('f');

        nums[3] = temp;

        temp = new ArrayList<>();
        temp.add('g');
        temp.add('h');
        temp.add('i');

        nums[4] = temp;

        temp = new ArrayList<>();
        temp.add('j');
        temp.add('k');
        temp.add('l');

        nums[5] = temp;

        temp = new ArrayList<>();
        temp.add('m');
        temp.add('n');
        temp.add('o');

        nums[6] = temp;

        temp = new ArrayList<>();
        temp.add('p');
        temp.add('q');
        temp.add('r');
        temp.add('s');

        nums[7] = temp;

        temp = new ArrayList<>();
        temp.add('t');
        temp.add('u');
        temp.add('v');

        nums[8] = temp;

        temp = new ArrayList<>();
        temp.add('w');
        temp.add('x');
        temp.add('y');
        temp.add('z');

        nums[9] = temp;

        return nums;
    }

}
```
