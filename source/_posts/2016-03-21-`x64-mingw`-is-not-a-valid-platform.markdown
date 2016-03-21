---
layout: post
title: "`x64_mingw` is not a valid platform"
date: 2016-03-21 07:38:40 -0700
comments: true
categories: bundle bundler rails
---
This is an annoying error, which I experienced while trying to set up a project in the beta Rails 5 (api mode). If you experience it, try seeing what bundler version you have:
```
$ bundle --version
#=> outputs something like 1.7.7
```

It could be that you need to update your version.

```
gem update bundler
```

That fixed it for me.