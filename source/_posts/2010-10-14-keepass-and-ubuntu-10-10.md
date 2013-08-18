---
title: Keepass and Ubuntu 10.10
author: ogrim
layout: post
permalink: /2010/10/keepass-and-ubuntu-10-10/
comments: true
---
There is a issue with KeePass in Ubuntu 10.10. As I had to fix this on several computers, I am posting the solution here. It was somewhat [inconvenient to track down][1].

Run this command in the terminal:
``` bash
sudo apt-get install libmono-winforms2.0-cil libmono-system-runtime2.0-cil
```

Earlier you only needed to install the winforms package to run KeePass, but with Ubuntu 10.10 it seems you need the system-runtime as well.

 [1]: http://sourceforge.net/projects/keepass/forums/forum/329221/topic/3888372
