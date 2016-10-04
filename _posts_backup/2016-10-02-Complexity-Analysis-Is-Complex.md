---
layout: post
title: Complexity Analysis is somehow Complex
categories: [algorithm]
tags: [algorithm, complexity-analysis]
description: Analyze time complexity of a solution to 132 pattern problem, and how time complexity is different from what I've thought.
---

There is a problem, called "132 pattern" in Homework1 of CSE202. Here is the description:


##### 132 Pattern

<dl>
Given a sequence of n distinct positive integers a[1], . . . , a[n], a 132-pattern is a subsequence a[i], a[j], a[k] such that i < j < k and a[i] < a[k] < a[j] . For example: the sequence 31, 24, 15, 22, 33, 4, 18, 5, 3, 26 has several 132-patterns including 15, 33, 18 among others. Design an algorithm that takes as input a list of n number and checks whether there is a 132-pattern in the list
</dl>

Naive solution takes O(n^2) time, which is pretty obvious. Somehow it is also quite easy to come to a much better solution. Since we are going to find a nubmer that lies between previous larger value a[j] and previous smaller value a[i], this problem looks similar to a interval problem, which goes to :

##### Similar problem
<dl>
Given a sequence of n intervals, find if they overlap.
</dl>


At the first glance to this similar problem, we might come to the conclusion that this is a O(n) problem by instinct, because many if-exist problem have O(n) solutions. So after some consideration, we came to this better solution:

#####Better solution
    * Store all the intervals formed by previous smaller numbers and larger numbers into a stack.
    * For every new number:
        * Find if it belongs to some interval.
            * If it belongs to an interval, return true.
            * If not,