---
title: Opening Spotify URIs in Spotify Linux Preview from Opera
author: ogrim
layout: post
permalink: /2011/11/spotify-uri-linux/
comments: true
---
Using Spotify Linux Preview, I want to open URIs to songs and playlist from Opera. This was not working, so here is how you fix it:

Stick the following in a file:

``` bash
#!/bin/sh
spotify -u $@
```

Make it executable with `chmod +x`

Go to Opera preferences by pressing `Alt+P` or `Ctrl+F12`, then go to `Advanced`, `Programs`, press `Add`, in `Protocol`, enter `spotify` and put the path to the script in `Open with other application`. Now you can open all the playlists!
