---
layout: post
title: "A Taste Of Metaprogramming: Method_Missing"
date: 2014-08-06 21:35:02 -0400
comments: true
categories: 
---

Anyone with a background in Ruby has seen a hash before. And anyone with a background in Javascript has seen a Javascript Object before. I encountered an interesting problem on CodeWars.com and wanted to share. The problem deals with accessing data in Ruby hashes the way you can in Javascript objects - by using the dot notation.

<!--More-->

Here's how I want to access the values:

hash = {a: 2, "b": 1, c: 0.5}

hash.a # must return the same as hash[:a]
hash.b # must return the same as hash["b"]
hash.size # size method must stay intact and return 3
hash.q # must return NoMethodError

###Approach

First, since we're trying to modify the way we can access data from a hash, we must change the Hash class.

The third of the four constraints was particularly helpful in helping construct a solution because it meant that one couldn't overwrite any of the existing methods in the Hash class - calling methods such as size should result in their normal Hash functionality.

Therefore, it seemed appropriate to use the `method_missing` method. This was the first time I'd encountered it.

###What is method_missing

RubyLearning.com defines method_missing quite well:

>Whenever you send a message to an object in Ruby, the object executes the first method it finds on its method lookup path with the same name as the message. If it fails to find any such method, it raises a NoMethodError exception - unless you have provided the object with a method called method_missing. The method_missing method is passed the symbol of the non-existent method, an array of the arguments that were passed in the original call and any block passed to the original method.

In order to harness the power of method_missing, we must reopen the Hash class and get to work. Here was my initial solution:

```ruby
class Hash
  
  def method_missing(name)
    if self.keys.any? {|key| key == name.to_s || key == name.to_sym}
      if self.keys.include? name.to_s
        self[name.to_s]
      else
        self[name.to_sym]
      end
    else
      raise NoMethodError
    end
  end

end
```
While this works, it can be shortened significantly. Specifically, we won't need to use a large if/else statement with lots of conditions if we know a little about what ruby will attempt to do.

We know that when a method is called on our hash that is also the name of a key in the hash, we want to retrieve that key's value. Here is a refactored solution.

```ruby
class Hash

  def method_missing(name)
    self[name.to_sym] || self[name.to_s] || super
  end

end
```

We know that the first of three expressions on line 4 of the refactored solution will work unless the key is a string. That's why we set up a chain of `||` "or" operators. If the key is a string, the first expression will return nil, and the truthiness of nil is false. Therefore, Ruby proceeds to the second funtion, and finds the correct key!

Lastly, we include the `super` expression at the end. We could have specified a `raise NoMethodError`, but using super is slightly more flexible and taps into the behavior of the Hash class. Cool!

###A Note on Duck-Punching / Monkey-Patching

While this is a useful exercise and requires an understanding of method_missing to solve, it is somewhat frowned upon to alter the core of Ruby. Someone reading my code might know what's going on if it were on one page, but it's confusing for everyone else. Also, there might be upgrading problems; if something about the class changes between versions your particular monkey-patch might fail. 


