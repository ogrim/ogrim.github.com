---
title: stumpwm and cmus (or any other terminal program)
author: ogrim
layout: post
permalink: /2011/05/stumpwm-cmus/
---
**Update 2nd September 2011:** It seems I forgot to write how to configure gnome-terminal to keep the original title. In *Profile Preferences* go to *Title and Command* and set *When terminal commands set their own titles* to *Prepend initial title*.

[Stumpwm][1] is an excellent tiling window manager written entirely in Common Lisp. It is fully keyboard driven and hackable to the fullest. The wm is running in it&#8217;s own CLisp REPL, so you can connect with Slime+Emacs and hack directly. It is super easy to customize the wm, simply re-evaluate and you are good to go!

[cmus][2] is an equally awesome program I enjoy using, a very efficient music player that runs in a terminal emulator.
<img src="http://ogrim.no/wp-content/uploads/cmus-2.3.0.png" alt="" title="cmus-2.3.0" width="660" height="436" class="aligncenter size-full wp-image-369" />
I wanted to have a keybinding either bringing cmus in focus, or starting it if it isn&#8217;t running. As cmus is running in a terminal emulator, it wasn&#8217;t straight forward how to achieve this. I am using the Gnome Terminal, so this solution will have to be modified to work with other terminals. Of course, the code is added to the `.stumpwm` file, but you could also just evaluate it to try it out.

	(defcommand cmus () ()<br />
	  "Run or switch to CMus"<br />
	  (run-or-raise "gnome-terminal -e cmus -t cmus"<br />
	(:instance "gnome-terminal" :title "cmus")))

Now I just have to bind the command to an appropriate key

	(define-key *root-map* (kbd "C-p") "cmus")

This works because I set the title of the terminal when I launch it. cmus changes the title based on what song is playing, so it will not work without doing this.

 [1]: http://www.nongnu.org/stumpwm/
 [2]: http://cmus.sourceforge.net/
