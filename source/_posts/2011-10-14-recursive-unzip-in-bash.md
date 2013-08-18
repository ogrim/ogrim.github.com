---
title: Recursive unzip in bash
author: ogrim
layout: post
permalink: /2011/10/recursive-unzip-in-bash/
comments: true
---
There is a lot of data I need to extract from archives, nested deeply within a silly folder structure. Doing it manually is out of the question. I had to modify this script I found at the [Unix Stack Exchange][1] slightly, in order to make it run as I wanted. It will run until it doesn&#8217;t find more .zip archive files, **as it deletes them after extraction**.

I put this into a file:

``` bash
#!/bin/bash
shopt -s globstar nullglob
while set -- **/*.zip; [ $# -ge 1 ]
do
    for z
    do
        ( cd -- "$(dirname "$z")" &&
            z=${z##*/} &&
            unzip -- "$z" &&
            rm -- "$z"
        )
    done
done
```

then made it executable with `chmod +x` and stuck it on my path. Now I can easily unzip all the archive files recursively, from the folder where I call the script.

 [1]: http://unix.stackexchange.com/questions/4367/extracting-nested-zip-files
