---
layout: post
title: dos2unix on Mac
categories: []
tags:
- Bash
- File formats
- OSX
status: publish
type: post
published: true
meta:
  _edit_last: '1'
author:
  login: admin
  email: niklas.mahler@gmail.com
  display_name: Niklas
  first_name: Niklas
  last_name: Mähler
---
In Linux there's a nice little tool called `dos2unix` that converts those 
nasty Windows line breaks into Unix line breaks. This is not available in OSX, 
but it is possible to do it with [`tr`][1]:

{% highlight bash %}
tr '\r' '\n' < file_to_convert.txt > output.txt
{% endhighlight %}

Nice and simple, and it seems to work just fine.

[1]: http://developer.apple.com/library/mac/#documentation/Darwin/Reference/ManPages/man1/tr.1.html
