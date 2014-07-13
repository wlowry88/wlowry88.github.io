---
layout: post
title: "Giving Pry a Try"
date: 2014-07-13 19:25:44 -0400
comments: true
categories: 
---

First - REPLs.

What is a REPL? It stands for read-eval-print-loop, and is also known as an interactive top-level or language shell. It’s super easy to use for beginners, which is part of what has made Code School and Codeacademy so successful. 

We also have experience with REPL’s - for example, our old friend IRB. IRB is pretty cool, but doesn’t really have too many features. As we know, sometimes it messes up the indentation, it gets messy, copying and pasting is funky (“Do you really want to display all 932309423 items?” anyone??”).

Pry is super easy to install. gem install pry pry-doc.

You can actually invoke it right from the command line. Just type “pry”. 
.pryrc can make customizations. Already so cool!

they have special set of commands - you can type help. help -h. So you could type gist-method -h. So now you can see the help options. 

s = “pry” <- a string. Pry recognizes that it’s a string - color codes.

show-doc is a really awesome feature - shows the doc on the objects. So first, show-doc. show-doc String#each_line.

Or show-method s.each_line. It shows the implementation for each line using String. -l for line numbers. How nice! To get it out, just use the gist-method s.each_line, and it has turned it into a github gist already! How cool. 

require ‘hello_world’
ls will list out methods and variables. ls HelloWorld. The capital -M flag will list out instances. ls HelloWorld -m. ls HelloWorld -mj. 
In pry, you can cd into and out of objects. How cool is that. cd HelloWorld. ls -mj. It knows implicitly that we’re in HelloWorld. 

So when you’re interested in the output instead of the return value, you can put a ; after. 

or we can do gm-cd hello_world.
.pwd
.tree (shows how the gem looks). 
.cat hello_world.rb. 


cd String
cd Regexp

nesting. cd .. or jump-to 0. 

def helo(name)
show-input
! (clears it all out)

def hello(name)
     “Hello, #{name}
amend-line 2 “Hello, #{name}”



Pry knows where our methods come from. binding.pry in the middle gives you access to local variables. 
whereami

edit-method each

show-method each

You can completely debug using Pry.







add pry as a gem in the gem file. :group => :development
bundle install
config environments development.rb, and replace IRB with Pry. silence_warnings do / require ‘pry’/ IRB = Pry / end
rails console using Pry. 

cd Category ls -M, ls -i. So we have access to these. @_destroy_callbacks

