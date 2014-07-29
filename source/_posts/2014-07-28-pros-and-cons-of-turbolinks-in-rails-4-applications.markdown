---
layout: post
title: "Pros and Cons of Turbolinks in Rails 4 Applications"
date: 2014-07-28 22:02:31 -0400
comments: true
categories: 
---

###Inspiration
Recently, I was playing around with jQuery and AJAX since we'd been learning about how to use event listeners and asynchronous Javascript requests. I made a blogging app that can add and edit posts and comments on those posts using AJAX. Additionally, I tried to add in a few event listeners using jQuery, such as using a slideToggle function on showing the full text of a post or the add comment form. 

Everything was going great; refreshing the posts index page yielded all my nice front-end effects. Until I tried linking to it. 

THEN EVERYTHING BROKE.

<!--More-->

###What are turbolinks?

If you don't know what they do, it's not that complex. Basically, turbolinks will capture all links that look like they go to HTML pages (links using restful routes are prime candidates), makes an AJAX request for the content, and then replaces the body with the response's body. It has the potential to speed up your app a little bit. Steve Klabnik wrote a [post](http://blog.steveklabnik.com/posts/2012-09-27-seriously--numbers--use-them-) about testing speed differences. The difference can be measurable - using Basecamp Next's CSS and JS files, turbolinks were able to decrease testing time by about 11 seconds. Not bad. 

###The Problems

The first problem is that Turbolinks do not call document’s ready event. Say you wanted to use Javascript's alert function on a page, or if you wanted to hide an element as soon as the page is loaded (perhaps to be shown later). 

```js
alert("You're seeing me if this page was directly loaded");
// a popup window
$("#thing-we-wanted-hidden").hide();
// won't work on a link.
```
The code above will only work when the page is refreshed directly. Unfortunately, when a page is accessed via a link in Rails 4, the turbolinks gem doesn't fetch it. Note, there is a workaround for this particular issue (jquery-turbolinks) but it's still a little annoying. And it doesn't address another issue.

Turbolinks can speed things up a little because it doesn't need to reload your assets. But it's more than just the assets that are saved. The entire global scope is saved. Yehuda Katz has said that "a lot of existing JavaScript operates under the assumption of a clean scope, and a single DOMContentLoaded event," and I tend to agree after my experience thus far. While it's possible to add workarounds (for example, I would re-apply most of my page-load jQuery effects in my Ajax-returned javascript), we're doing pretty simple jQuery so far and I can only assume that complications will increase when using more advanced third-party Javascript. 

###Ok, so how do you I rid of turbolinks in my app?

To remove turbolinks from your Rails 4 Application, you should follow 3 simple steps:

1. Delete the line `turbolinks` in your Gemfile to remove the turbolinks gem.

2. Delete the line `//= require turbolinks` in your app/assets/javascripts/application.js file to stop the asset pipeline from trying to require turbolinks.

3. Delete both lines `data-turbolinks-track => true` key/value pairs in your app/views/layouts/application.html.erb file to stop including a now-invalid hash option.

###Conclusion

If you're trying to include a good amount of Javascript, jQuery [Turbolinks](https://github.com/kossnocorp/jquery.turbolinks) seem to be more trouble than they're worth. 

