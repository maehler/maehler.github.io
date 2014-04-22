---
layout: post
title: "Interrupting an SSH session"
description: ""
category: 
tags:
  - bash
---

I spend the majority of my days connected to remote servers via SSH. It's a
pleasant way of working in many aspects, but since I arrived in the US I've
experienced my fair share of bad networks. At some points I've been thrown out
from the network once a minute. In most cases, SSH is able to maintain the 
connection, but sometimes I have to reconnect. Unfortunately, SSH does not detect
this right away. It just stands there, waiting for the connection to come back
and doesn't respond to signals while doing it.

Instead of closing the terminal window to speed up the restart of the connection,
I have now found out that it is possible to close the connection using SSH
escape sequences. In this case press Enter and then `~.` to terminate the session.
With `~?` you can see other available escape sequences.
