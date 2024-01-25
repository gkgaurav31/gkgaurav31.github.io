---
layout: post
title: Highest Score (InterviewBit)
date: 2024-01-25 21:58 +0530
author: "Gaurav Kumar"
tags: "java mathematics"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

You are given a 2D string array A of dimensions N x 2,
where each row consists of two strings: first is the name of the student, second is their marks.
You have to find the maximum average mark. If it is a floating point, round it down to the nearest integer less than or equal to the number.

## SOLUTION

This is pretty simple problem. It becomes easier if we create a Student class to store the details needed to calculate the average later.

```java
public class Solution {

    public int highestScore(String[][] A) {

        int n = A.length;

        Map<String, Student> map = new HashMap<>();

        for(int i=0; i<n; i++){

            String studentName = A[i][0];
            Double studentMarks = Double.parseDouble(A[i][1]);

            if(map.containsKey(studentName)){

                Student student = map.get(studentName);
                student.marks  = student.marks + studentMarks;
                student.subjectCount = student.subjectCount + 1;

            }else{

                Student student = new Student();
                student.name = studentName;
                student.marks = studentMarks;
                student.subjectCount = 1;

                map.put(studentName, student);

            }

        }

        int highestAverage = 0;

        for(Student student : map.values()){

            Double totalMarks = student.marks;
            int subjects = student.subjectCount;

            int averageMarks = (int)(totalMarks/subjects);

            highestAverage = Math.max(highestAverage, averageMarks);

        }

        return highestAverage;

    }

}

class Student{
    public String name;
    public Double marks;
    public int subjectCount;
}
```
