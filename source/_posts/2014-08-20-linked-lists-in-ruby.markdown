---
layout: post
title: "Linked Lists in Ruby"
date: 2014-08-20 23:51:07 -0400
comments: true
categories: Ruby Linked Lists CS
---

This post is centered around linked lists using Ruby. 

A linked list is a **data structure** that consists of a collection of nodes that represent a sequence. Each element in a linked list will contain a datum and a reference to the next element in the linked list (a pointer).

In Ruby it makes most sense to use arrays due to built-in methods such as `shift`, `unshift`, `enq`, `deq`, `push` and `pop`, but it is helpful to know why linked lists can be beneficial.

<!-- More -->

Linked lists' biggest advantage over arrays in other languages is their ability to insert / remove list elements without reallocating or reorganization of the entire data structure. Arrays have indices, so deleting a value at index 0 for example requires every single item to be reindexed.

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

It makes sense to define two ruby classes in implementing a linked list in Ruby - one for each element, called `Node`, and the other for the list itself, called `LinkedList`.

Here is the first. Note that a Node is initialized with its own data point as well as a pointer to the next Node.

```ruby
class Node
	attr_accessor :val, :next

	def initialize(val, next_node)
		@val = val
		@next = next_node
	end
end
```
Next, the LinkedList class:

```ruby
class LinkedList

  def initialize(val)
    @head = Node.new(val, nil)
  end

  def add(val)
    current = @head
    while current.next != nil
      current = current.next
    end
    current.next = Node.new(val, nil)
  end

  def delete(val)
    current.next = @head
    if current.val = val
      @head = current.next
    else
      while (current.next != nil) && (current.next.val != val)
        current = current.next
      end
      unless current.next == nil
        current.next = current.next.next
      end
    end
  end

  def return_list
    elements = []
    current = @head
    while current.next != nil
      elements << current
      current = current.next
    end
    elements << current
  end
end

```

And there you have it - a basic implementation of a singly linked list in Ruby. Next time we'll use a linked list to build a stack and a queue. 
