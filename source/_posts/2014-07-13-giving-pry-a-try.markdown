---
layout: post
title: "Using Pry for More Efficient Debugging"
date: 2014-07-13 19:25:44 -0400
comments: true
categories: 
---

We were in lecture here at the Flatiron School the other day, and Avi mentioned something called a REPL. He also mentioned that one of our favorite debugging tools, Pry, had a lot of cool functionality that we don't often exploit, so I decided to dig around a little bit and see what I could find. 
<!-- More -->

###**REPLs**

What is a **REPL**? The acronym itself stands for "**r**ead-**e**val-**p**rint-**l**oop," and describes an environment where you type in code and get interactive feedback immediately. This type of tool is also known as an interactive top-level or language shell. It’s super easy to use for beginners, which is part of what has made Code School and Codeacademy so successful. No text editors, just instant feedback. For example, while making my Tic-Tac-Toe, I used [this online REPL extensively](http://repl.it/languages/Ruby). When you're at the earliest stage of learning to code this is a great tool. However, REPL's are also very useful for full-fledged developers.

```ruby
#Some random REPL output:
2 + 2
#=> 4
```

We also have experience with REPL’s - such as our friend **IRB** (Interactive Ruby!). IRB is pretty cool for interpreting your Ruby on the spot, and it's definitely useful, but it also doesn’t really have too many features. And sometimes, it's a little visually confusing; anyone who has ever tried to define a method in IRB knows that it messes up the indentation a bit, for instance. There's definitely no syntax highlighting, and it's not really that useful from a debugging standpoint. In order to access the methods from your actual program, for instance, you're often stuck copying and pasting, which gets funky pretty quickly. You'd BETTER have converted from tabs to spaces before that. And also, you get annoying (“Do you really want to display all 9423 items?” anyone??”)-type messages.

###**Enter 'Pry'**

Pry is d0pe. It works as a powerful alternative to the standard IRB shell for Ruby. It features _syntax highlighting_, a _flexible_plugin_architecture_, _runtime_invocation_ and _source/documentation_browsing_! Us Flatiron School Ruby 005's have already used it for debugging purposes, but it can do a **whole lot more**.

Pry is super easy to install: `gem install pry` does the trick. Like IRB, Pry can be invoked right from the command line. **To start it, just type “pry”.** Then you will be in something that looks and feels like IRB, but is better in a lot of ways! For example, all the indentation works well, and you have syntax highlighting. **You're able to customize all of this as well in your ".pryrc" file.** This is analogous to the IRB ".irbrc" file. For example, here is a way to make it so that your pry output uses awesome print:

```ruby
# ==============================
#   Awesome Print
# ==============================
# Pretty print your Ruby objects with style -- in full color and with proper indentation
# http://github.com/michaeldv/awesome_print
if defined? AwesomePrint
  AwesomePrint.pry!
  ## The following line enables awesome_print for all pry output,
  # and enables paging using Pry's pager with awesome_print.
  Pry.config.print = proc {|output, value| Pry::Helpers::BaseHelpers.stagger_output("=> #{value.ai(indent: 2)}", output)}
  ## If you want awesome_print without automatic pagination, use below:
  # Pry.config.print = proc { |output, value| output.puts value.ai }


  ## Evaluated result display inline
  # Pry.config.print = lambda { |output, value| output.print "\e[1A\e[18C # => "; output.puts value.inspect }

  ## if in bundler, break out, so awesome print doesn't have to be in Gemfile
  if defined? Bundler
    Gem.post_reset_hooks.reject! { |hook| hook.source_location.first =~ %r{/bundler/} }
    Gem::Specification.reset
    load 'rubygems/custom_require.rb'
  end

  ## awesome_print config for Minitest.
  if defined? Minitest
    module Minitest::Assertions
      def mu_pp(obj)
        obj.awesome_inspect
      end
    end
  end
end 
```

###**TONS OF AWESOME FEATURES**

First, you can get to the entire Ruby documentation from **inside pry**. This is really awesome, because it means that we can be lazier and avoid the treachorous Google path and not have to leave our shell. For example:
```
pry
[1] pry(main)> 
[2] pry(main)> show-doc Array#each

From: array.c (C Method):
Owner: Array
Visibility: public
Signature: each()
Number of lines: 11

Calls the given block once for each element in self, passing that element
as a parameter.

An Enumerator is returned if no block is given.

   a = [ "a", "b", "c" ]
   a.each {|x| print x, " -- " }

produces:

   a -- b -- c --
```
Pretty cool, right? And for those people who are curious how a method based in C works, you can also get to the source code of the method really easily!
```
[3] pry(main)> 
[4] pry(main)> show-method Array#each

#=>
From: array.c (C Method):
Owner: Array
Visibility: public
Number of lines: 12

VALUE
rb_ary_each(VALUE array)
{
    long i;
    volatile VALUE ary = array;

    RETURN_SIZED_ENUMERATOR(ary, 0, 0, ary_enum_length);
    for (i=0; i<RARRAY_LEN(ary); i++) {
	rb_yield(RARRAY_AREF(ary, i));
    }
    return ary;
}

```
Wow!! And you can also totally get line numbers with the `-l` option.

You can also get a Github gist of any method! Once you install the `gist` gem, you can totally generate a gist with one command:

```
[1] pry(main)> gist -m Symbol#to_proc
[2] Gist created at URL https://gist.github.com/381e426ba5a0307066b3, which is now in the clipboard.
```
Sweet. Github just created a Gist of your code.

Pry supports a bash-like method of navigation. When you're in Pry, you can type **"whereami"** at any time, and it works a lot like the **pwd** command in bash. You can type **ls** to see the directories and files in your current level of Pry, and can even "cd" into them. 

###Practical Use!
So let's actually use it a little bit. Here, we'll use it to help improve our Sailor model.

```ruby

  require 'pry' #remember to require the 'pry' gem.

  class Sailor
      attr_reader :name

      def initialize(name)
          @name = name
      end

      def say_hello_to_everyone
          put "#{self.name} at your service, Captain!"
      end

      def go_sailing
          puts "Weeeeeeeeeeee"
      end
      binding.pry         #<= We stick 'binding.pry' into our code to stop it there.
  end

```
And then your binding stops your code in its tracks! It keeps track of your line numbers and stops you exactly where you want it to.
```
From: /Users/wlowry/Desktop/sailor.rb @ line 18 :

    13: 
    14:   def go_sailing
    15:     puts "Weeeeeeeeeeee"
    16:   end
    17:   
 => 18:   binding.pry         #<= We stick 'binding.pry'
    19: end

[1] pry(Sailor)> ls -M
=> Sailor#methods: go_sailing  name  say_hello_to_everyone

[2] pry(Sailor)> arel = Sailor.new("Arel")
=> #<Sailor:0x0000010222d7d0 @name="Arel">

[3] pry(Sailor)> arel.say_hello_to_everyone
NoMethodError: undefined method `put' for #<Sailor:0x0000010222d7d0 @name="Arel">
from sailor.rb:11:in 'say_hello_to_everyone`

[4] pry(Sailor)> wtf
#Exception: NoMethodError: undefined method `put` for #<Sailor:0x000001028a8768 @name="Arel">
--
#0: sailor.rb:11:in `say_hello_to_everyone`
#1: (pry):3:in `<class:Sailor>'
#2: /Users/wlowry/.rvm/gems/ruby-2.1.2/gems/pry-0.10.0/lib/pry/pry_instance.rb:353:in `eval'
#3: /Users/wlowry/.rvm/gems/ruby-2.1.2/gems/pry-0.10.0/lib/pry/pry_instance.rb:353:in `evaluate_ruby'
#4: /Users/wlowry/.rvm/gems/ruby-2.1.2/gems/pry-0.10.0/lib/pry/pry_instance.rb:321:in `handle_line'

[5] edit say_hello_to_everyone
```
###Breakdown of what just happened

1) Wow, `binding.pry` stopped me in the middle of my code. Now I can run `ls` as if I were in a bash terminal, and I can also use the `-M` option to specify that I want to see all the methods of the class. Wow! It returns them right there for me to see (Note: if you do this in Rails, you'll realize how much the framework gives you, it's nuts).

2) Oh cool, I can instantiate new instances of my Sailor class right there on the spot; I don't need to copy and paste my class into my REPL and waste time. We don't have a console in this application, so this is very useful.

3) Lets try exercising the full power of our sailor, Arel. When we call `say_hello_to_everyone` on our swabber, we get an error. It looks like there's a breakdown in line 11: there is no method `put` for Arel. Well, that makes sense, because it was a typo!

4) If you type in `wtf` anywhere in pry, it returns the last error message that was logged (it keeps track of your history, accessible with the `history` command):D.

5) Wow, so I need to edit that "say_hello_to_everyone" method. But I can totally just do that by typing in `edit say_hello_to_everyone`. And then you know what it does? It sends me right over into Sublime, right to the CORRECT LINE where the method is defined. HOW COOL IS THAT?

### Conclusion

So pry is pretty awesome - at the minimum it's better at doing most of the things you need than IRB. And I know what you're thinking - "it would be so awesome to include this in my rails projects and run pry instead of the rails console." Guess what: You TOTALLY CAN!

Add this line to your Gemfile:

```ruby
gem 'pry-rails', :group => :development
```
Once you update your Gemfile and run `bundle install`, you're ready to enjoy Pry and all of the benefits it brings. You'll be well on your way to understanding how your code works and debugging uber efficiently, without headaches, and with color syntax highlighting :)

Until next time, enjoy Pry!

