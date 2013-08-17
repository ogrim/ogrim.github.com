---
title: Keepass and Ubuntu
author: ogrim
layout: post
permalink: /2011/06/keepass-and-ubuntu/
---
Regarding my last recommendation for Ubuntu and Keepass, there is now a much simpler option: a `ppa-repository`. Installation now only requires 3 commands, and no fiddling with setting up a script to launch the .exe-file with mono.


	sudo apt-add-repository ppa:jtaylor/keepass
	sudo apt-get update
	sudo apt-get install keepass2

`Alt+F2`, `keepass2`, `Enter`, and be on your way. In my experience, this also much more stable than running Keepass with mono. This is quite old news and more accessible than my last post on the matter, as [this information][1] is linked to from the [main Keepass site][2].

 [1]: http://sourceforge.net/projects/keepass/forums/forum/329220/topic/4503818
 [2]: http://keepass.info/
