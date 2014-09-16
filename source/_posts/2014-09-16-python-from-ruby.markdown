---
layout: post
title: "Python from Ruby"
date: 2014-09-16 16:34:09 -0400
comments: true
categories: Python Ruby Languages
---

Recently, I've been learning a lot more about Python. I'm a Rubyist by background but I'm interested in learning Python because of all it's amazing libraries, especially those related to data science. This post is about a few differences that are useful to think about coming from a Ruby background. This is also largely a reaction to material I've gathered from the web, including [this](http://stackoverflow.com/questions/4769004/learning-python-from-ruby-differences-and-similarities) thread on Stack Overflow. 

<!--More-->

###A Few Differences So Far

**Philosophy**. It seems that Python and Ruby were conceived with slightly different aims. While both are high-level, interpreted, general programming languages, Python is very focused on being explicit. I only just learned about this easter egg in Python:

```python
>>> import this

"""The Zen of Python, by Tim Peters

Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren't special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
Now is better than never.
Although never is often better than *right* now.
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea -- let's do more of those!"""
```
When I was learning Ruby, I remember being blown away by the number of ways to solve any given problem. Yukihiro Matsumoto, the creator of Ruby once said, "I hope to see Ruby help every programmer in the world to be productive, and to enjoy programming, and to be happy. That is the primary purpose of Ruby language." I don't know about you, but it seems pretty clear that Ruby is intended to be programmer-centric. If there are two Rubyists who choose to solve a problem two different ways, let them! This stands in sharp contrast to several items from the Python "creed" - especially "There should be one-- and preferably only one --obvious way to do it." Python is not programmer-centric, it's program-centric.

**Ruby has blocks, Python does not.**

**Python has functions, Ruby does not.** In Python you can pass a function or method and pass it to another function, whereas in Ruby, everything is a method and you need to wrap them in Procs to pass them.

**Scope/Closures**. In Ruby, closures are defined using blocks - within a block you have full read and write access to variables defined outside the block. In Python, you can define functions inside of other functions but you don't have write access to variables from the outer function, only read access.

**Python uses list comprehensions, which are pretty awesome.** If you want to do an operation with a list, it's often easier in Python. For example, maybe you wanted to return a new list of all the square numbers of elements greater than 10, you could do this in Python:
```python
my_list = [1, 5, 9, 11, 12, 13]

[x**2 for x in my_list if x > 10]

#=> [121, 144, 169]
```
That's pretty expressive. In Ruby, the following would be a good solution but it's just less easy to read:
```ruby
my_list = [1, 5, 9, 11, 12, 13]

my_list.select { |item| item > 10 }.collect { |value| value ** 2 }

#=> [121, 144, 169]
```
Obviously both work and return the same value - but Python does it in one pass instead of having to hold 3 arrays in memory.

**Python has tuples, Ruby uses arrays to simulate tuples.** Maybe someday soon I'll appreciate this difference.

**Ruby has case statements, Python doesn't.** I don't really care too much about this one.

**Ruby has ternary operators, Python doesn't.** require_ternary_operators_to_stay_sane?? Ruby : Python

**Python has multiple inheritance, Ruby uses modules and mix-ins.** This is actually pretty important, and I think I prefer the module approach. 

**Python only has single-line lambda functions.** Ruby has a quasi-equivalent with the block, and so Ruby code becomes more functional as a result - I tend to gravitate towards the `each` and `map` methods in Ruby but definitely go more for the `for x in y` approach in Python. However, there is a nice equivalent for the `map` method in Python. Given a Python function, you can also do things functionally instead of with the list comprehension:

```python
def my_method(x):
		return x + 4

my_list = [1,2,3]

map(my_method, my_list)

#=> [5, 6, 7]
```

**NumPy, SciPy, Scikit-learn**. Python has really cool libraries for data science, which is what I might be interested in down the line.

###Conclusion:

There's no winner here. They're both incredible languages and I'm still a whitebelt in both. However, there's definitely value to learing both - they both teach slightly different ways of attacking problems, both have great web solutions (I think Rails still has Django beat right now), and both are a distinct pleasure to work with.

Tata for now.

