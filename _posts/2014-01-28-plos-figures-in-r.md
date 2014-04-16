---
layout: post
title: PLOS figures in R
categories: []
tags: []
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
I'm preparing a manuscript for PLOS ONE and saw this in the figure guidelines:

>Figure text must be in Arial font, between 8 and 12 point.

Easy, I thought. Just a matter of specifying a font family in the device I print to.

Think again.


Apparently, R does not support using different font definitions. Of course I'm 
not the first person to encounter this problem. In an [excellent post by 
Gavin Simpson][1] he explains how to come around this, and even to embed fonts 
in PDFs printed by R. In short, take a look at the [`extrafont` package][2].
It enables you to use fonts on your system in your R figures.

To get this to work properly, I had to specify one single directory to look 
when importing the fonts in since I apparently had multiple copies of Arial 
on my system (Mac OSX 10.9). This can be done easily by using 
`font_import(path=c('/Library/Fonts'), recursive=FALSE)` to ensure that only 
one copy is used.

[1]: http://www.fromthebottomoftheheap.net/2013/09/09/preparing-figures-for-plos-one-with-r
[2]: http://cran.r-project.org/web/packages/extrafont/index.html
