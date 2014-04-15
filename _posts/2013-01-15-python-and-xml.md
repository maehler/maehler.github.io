---
layout: post
title: Python and XML
categories: []
tags:
- File formats
- Python
status: publish
type: post
published: true
meta:
  _edit_last: '1'
  _wp_old_slug: '240'
author:
  login: admin
  email: niklas.mahler@gmail.com
  display_name: Niklas
  first_name: Niklas
  last_name: Mähler
---
Today I faced a task where I had to parse huge XML-files. And when I say huge, 
I mean 6-14 GB. My weapon of choice is Python, since I'm comfortable with 
it. However, I had never parsed XML with it before. Because of the size of the 
files, it was unfeasible to load the entire file into memory, and for me that 
was not necessary either.

After Googling for a while, I found that many people recommend the 
[ElementTree][1] module and it's C-equivalent cElementTree. The function 
[`iterparse`][2] proved to be a real life saver. By iterating through the 
element tree and deleting elements as you go, you will only consume small 
amounts of memory. The following snippet is more or less taken from 
[the documentation][3].

```python
import xml.etree.cElementTree as ET

# Get an iterable tree
context = ET.iterparse('huge_xml_file.xml', events=('start', 'end'))
# Get an iterator
context = iter(etree)
# Get the root element to be able to clear it
event, root = context.next()

# Iterate through the tree and to what you have to do
for event, elem in context:
    if event == 'start' and elem.tag == '{some_namespace}some_tag_name':
        # The element was opened here
    if event == 'end' and elem.tag == '{some_namespace}some_tag_name':
        # Now the whole element is available! Process it here.
        # When done with it, call
        root.clear() # to free up some memory
```

I don't like the way I had to specify the namespaces, but I guess there's a 
better way of doing it. When running this on a 6.4 GB XML file (161 million 
rows, 81 million elements), the code above did not consume more than 15 MB of 
memory. I don't remember how long it took, but it was reasonably fast.

[1]: http://effbot.org/zone/element-index.htm
[2]: http://effbot.org/zone/element-iterparse.htm
[3]: http://effbot.org/zone/element-iterparse.htm#incremental-parsing
