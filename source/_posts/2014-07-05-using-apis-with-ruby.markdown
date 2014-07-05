---
layout: post
title: "Using APIs with Ruby"
date: 2014-07-05 19:21:23 -0400
comments: true
categories: 
---

So everyone loves philosophical musings about beginning and learning, but I thought today’s post should be a little more technical. The topic - using API’s and getting data from other websites! This will be a two-part series - the first part will center around using API’s with Ruby and the second will be a surprise.

<!--More-->

##Part 1: Background on APIs with Ruby!

###HTTP

Everyone who’s used a computer with internet recently has already used HTTP. That’s how browsing the internet works. You internet browser sends an HTTP **request** to a server, and the server sends a **response** back to you.

When you navigate through a website, the links you’re clinking are all requests. And when you get to a different page with a different URL at the top, that’s a **state transition**; you have directed the next state of the application. HTTP is “**stateless**,” meaning that it has no memory across requests. 

So what the hell is an API, anyway? Isn’t that just like, a buzzword?

Well, yes. Because APIs are really important and get a lot of attention these days. API stands for **Application Programming Interface**, and is really like a contract; it specifies the ways you can interact with an application. So if you wanted to interact with an app like Twitter, for example, you’d need to use their API, which would specify how you authenticate, use URLs, classes, and other methods, etc.

###Making a Request

First, we _require_ the ‘open-uri’ module, which allows us to bring in the additional methods and constants we need for our HTTP requests. ‘open-uri’ is a very cool module that allows us to do a lot with URLs; later we'll be using it to scrape the internet. But for now, suffice it to say that it's very useful.

Here is an example that I found on CodeAcademy that is quite useful:

```ruby
require 'open-uri'

kittens = open('http://placekitten.com')

body = kittens.read[559, 441]

puts body

#=> "Meow Meow Meow"
```

###More about HTTP

Let's learn a little more about HTTP before we dive more into the thick of it. First, we'll learn about the four types of requests in HTTP. This is a tad misleading, because 

**The Four Verbs For HTTP**

GET - retrieves info from the source

POST - sends new info to the source

PATCH - updates information at the source

DELETE - removes information from the source 

**The Anatomy of a Request**

The _request line_: i.e. POST /willhasablog/yay HTTP/1.1

The _header_ i.e. Content-Type: text/html; charset=UTF-8

The _body_: i.e. Name=You

**Authentication & API Keys**

Most API’s require a key. The key accomplishes two things - not only does it grant you access, but it also identifies you TO the API - they like to know exactly how their service is used and prevent malicious activity. 
Keys look like this:

api_key = “FtHwuH8w1RDjQpOr0y0gF3AWm8sRsRzncK3hHh9"

###Responses

So we mentioned how every time you click on something on a site it’s actually a request to a remote server, right??
So when you make a successful request, like, when your question is heard, you will get a response. And the response will, among other things, contain a three-digit status code! Isn’t that awesome? Websites aren’t like people; they can't stonewall you. They respond! And the status code can start with a 1, 2, 3, 4, or 5. 

100s - More uncommon, server is working on your request.

200s - These mean ok- usually getting a 200 is the equivalent of things are fine.

300s - These mean that the site can do what you want but you might have to be rerouted to accomplish it (like a change in URL requiring a redirect).

400s - These are common - means that you probably made a mistake. 

500s - This means that the server goofed up and can’t successfully respond to the request.

**The Anatomy of a Response:**

Like a request, a response has three parts. 

The _response line_:i.e. HTTP/1.1 200 OK

The _header_: i.e. Content-Type: text/xml; charset=UTF-8
The _body_: i.e. <?xml version="1.0" encoding="utf-8"?>
# <string xmlns=“http://wlowry88.github.io">Accepted</string>

###XML and JSON

**WTF is XML?**

XML is another markup language that is human and machine readable used to encode documents. Here's how you might parse it:

```ruby
require ‘rexml/document’

file = File.open(“wlowrypost.txt”)
doc = REXML::Document.new file
file.close

doc.elements.each(“pets/pet/name”) do |element|
     puts element
end
```

**WTF is JSON?**

JSON stands for **J**ava**Script **O**bject**N**otation. It is based off of JavaScript objects, which look a lot like hashes to us Rubyists. It is more succinct than the equivalent XML, and it doesn’t require tags. 

```ruby
{
   "Cartoon Foxes": {
     {
      "Name": "Fox Tall",
      "Job": "Bein' tall"
     },
     {
       "Name": "Fox Small",
       "Job": "Bein' small"
     }
   }
}
```

**So, XML or JSON?**
The only way you’ll know what type of data you’ll get from an API is to read it’s documentation. 

###Phew, that was a lot.

Thanks so much, and next time we'll actually put what we learned today to use.

W