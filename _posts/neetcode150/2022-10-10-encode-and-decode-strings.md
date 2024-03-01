---
layout: post
title: Encode and Decode Strings
date: 2022-10-10 21:18 +0530
author: "Gaurav Kumar"
tags: "java neetcode150"
categories: "neetcode150"
---

This question is part of [NeetCode150 series](https://neetcode.io/practice).

## Problem Description

Design an algorithm to encode a list of strings to a string. The encoded string is then sent over the network and is decoded back to the original list of strings.
Implement the encode and decode methods.
You are not allowed to solve the problem using any serialize methods (such as eval).  
[leetcode](https://leetcode.com/problems/encode-and-decode-strings/)

### Solution

### APPROACH 1

One way to solve this problem is simply by using a delimeter. We will need to use a non-ASCII character as a delimiter since any ASCII character can be part of the original string.

Notice here that if all the strings in the input are empty, the split function will return an empty array which will give us an incorrect answer. To work-around this problem, we "mark" the empty input string using another delimeter d2.

```java
public class Codec {

    private static final String d1 = Character.toString((char)257);
    private static final String d2 = Character.toString((char)258);

    // Encodes a list of strings to a single string.
    public String encode(List<String> strs) {

        StringBuffer sb = new StringBuffer();

        for(String s: strs){
            if(s.equals("")){
                sb.append(s).append(d2).append(d1);
            }else
                sb.append(s).append(d1);
        }

        return sb.toString().substring(0,sb.length()-1);
    }

    // Decodes a single string to a list of strings.
    public List<String> decode(String s) {

        String[] arr = s.split(d1);

        System.out.println(Arrays.toString(arr));

        List<String> list = new ArrayList<>();

        for(int i=0; i<arr.length; i++){
            if(arr[i].equals(d2))
                list.add("");
            else
                list.add(arr[i]);
        }
        return list;
    }
}

// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.decode(codec.encode(strs));
```

### APPROACH 2

To understand the second approach, we need to understand the following:

1. How can we encode?
   ![snapshot]({{ site.baseurl }}/assets/img/encode_string.png)

2. How can we decode?
   ![snapshot]({{ site.baseurl }}/assets/img/decode_string.png)

```java

public class Codec {

    // Encodes a list of strings to a single string.
    public String encode(List<String> strs) {

        StringBuffer sb = new StringBuffer();
        for(int i=0; i<strs.size(); i++){
            String s = strs.get(i);
            sb.append(intToString(s));
            sb.append(s);
        }

        return sb.toString();

    }

    public String intToString(String s){

        int x = s.length();

        char[] bytes = new char[4];

        for(int i=3; i>=0; i--){
            bytes[i] = (char)((x >> (3-i)*8) & 0xff);
        }

        return new String(bytes);
    }

    // Decodes bytes string to integer
    public int stringToInt(String bytesStr) {
        int result = 0;
        for(char b : bytesStr.toCharArray())
            result = (int)(result << 8) + (int)b;
        return result;
    }

    // Decodes a single string to a list of strings.
    public List<String> decode(String s) {

        int n = s.length();

        List<String> decodedStrings = new ArrayList<>();

        int i=0;

        while(i<n){
            int length = stringToInt(s.substring(i, i+4));
            i+=4;
            decodedStrings.add(s.substring(i, i+length));
            i+=length;
        }

        return decodedStrings;

    }
}

// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.decode(codec.encode(strs));
```

### APPROACH 3

While encoding:

- For each word, append its length and an extra symbol like `#`.

While decoding:

- We can reach the character before `#` to get the length of next word. Then just iterate for the next characters defined by the length before `#` to get the word.

```java
public class Codec {

    public String encode(List<String> strs) {

        StringBuffer sb = new StringBuffer();

        // Iterate through each string in the list
        for(int i = 0; i < strs.size(); i++){

            // Append the length of the current string followed by a '#' delimiter
            sb.append(strs.get(i).length()).append("#");
            // Append the current string itself
            sb.append(strs.get(i));

        }

        return sb.toString();
    }

    public List<String> decode(String s) {

        int n = s.length();

        List<String> res = new ArrayList<>();

        int idx = 0;

        // Iterate through the input string
        while(idx < n){

            // StringBuffer to store the length of the next string
            StringBuffer currentLength = new StringBuffer();

            // Extract the length of the next string until '#' delimiter is found
            while(s.charAt(idx) != '#'){
                currentLength.append(s.charAt(idx) + "");
                idx++;
            }

            // Move past the '#' delimiter
            idx++;

            // Convert the extracted length to an integer
            int length = Integer.parseInt(currentLength.toString());

            // StringBuffer to store the characters of the current string
            StringBuffer currentString = new StringBuffer();

            // Extract the characters of the current string using its length
            for(int i = 0; i < length; i++){
                currentString.append(s.charAt(idx) + "");
                idx++;
            }

            // Add the decoded string to the result list
            res.add(currentString.toString());
        }

        return res;
    }
}
```
