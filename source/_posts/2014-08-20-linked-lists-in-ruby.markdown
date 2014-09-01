---
layout: post
title: "Linked Lists in Ruby"
date: 2014-08-20 23:51:07 -0400
comments: true
categories: Ruby Linked Lists CS
---

This post is centered around linked lists using Ruby. 

A linked list is a **data structure** that consists of a collection of nodes that represent a sequence. Each element in a linked list will contain a datum and a reference to the next element in the linked list (a pointer).

In Ruby it makes most sense to use arrays due to built-in methods such as `shift`, `unshift`, `enq`, `deq`, `push` and `pop`, but it is helpful to know why linked lists can be beneficial. Linked lists' biggest advantage over arrays in other languages is their ability to insert / remove list elements without reallocating or reorganization of the entire data structure. Arrays have indices, so deleting a value at index 0 for example requires every single item to be reindexed.

The flip-side of this, however, is that performing operations requiring access to particular elements of a linked list can be cumbersome. For example, finding the last element of a linked list requires scanning every element of the list. 

Here are some additional advantages and disadvantages of linked lists:

LL Pros:

- Dynamic - LL's allocate the needed memory when the program is initiated and can can expand in real time without memory overload.

- Insertion and deletion is easy.

- It's easy to building linear data structures such as stacks and queues with linked lists.

LL Cons:

- Since each element holds a value and a pointer, it's more memory intensive.

- You have to access nodes sequentially since that's how they're defined; furthermore they're not stored continuously so it takes longer to access an element. 

- With a singly-linked list it's pretty much terrible to reverse traverse. Doubly-linked lists contain a pointer to the previous node as well but that's even more memory intensive. 

###Ruby Implementation

It makes sense to define two ruby classes