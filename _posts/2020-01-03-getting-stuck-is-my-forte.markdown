---
title: "Getting Stuck is my Forte"
layout: post
date: 2020-01-03 22:48
# image: /assets/images/markdown.jpg
headerImage: false
tag:
- outreachy
- debian
category: blog
author: sakshisangwan
description: Everybody Struggles.
---

# Getting Stuck is my Forte \o/

I am so excited to write this blog, probably because this is the state where I am usually found, **stuck**. If you see me crying in a corner, I am in this afore mentioned state with the why’s and how’s. This is a part of my daily routine and thus couldn’t be separated from my Outreachy project as well. :P

I have been stuck on most of the tasks at most of the steps, I would love to share the one that was simplest and yet took me a lot of time.

## Task

>  Update Debian’s wiki for Gitlab with the latest package.json at [https://wiki.debian.org/Javascript/Nodejs/Tasks/gitlab](https://wiki.debian.org/Javascript/Nodejs/Tasks/gitlab)

**Details**
 https://wiki.debian.org/Javascript/Nodejs/Tasks/gitlab should not be manually updated, rather it contains a  [script](https://salsa.debian.org/js-team/js-task-wiki-edit) to generate the page. The python script  [js_task_edit.py](https://salsa.debian.org/js-team/js-task-wiki-edit/blob/master/js_task_edit.py "js_task_edit.py") takes in a file as an input and recursively fetches the dependencies of each mentioned package. It uses the tree module to create a dependency tree for the same.

- I had to parse Gitlab's [package.json](https://gitlab.com/gitlab-org/gitlab/blob/master/package.json "package.json") to this script.
- I was unable to figure out how to do the same. It took me hours of random hit and trials and asking folks around how to go about it.
- I cloned the above repository, passed the path to the package.json while running the python script. It didn't work, probably wasn't able to trace back to the path *(that's what I thought, apparently not true)*.
- I copied the package.json to the same directory as the script and passed the file as an argument. It still didn't work.
- It was pretty simple to do though, all that had to be done was run
```python js_task_edit.py -f package.json``` and the -f flag was what I was missing all this while.

It took hours *(my laptop died twice while running it)* for the final output to return since there are way too many package dependencies *(slow internet also seems a reasonable reason)*. There's still a pending task left here, I need to update the script to fetch dependencies up to 2-3 levels only. I have been trying a few ways that don't seem to have worked up till now. (I'll write more about it when I seem to have some progress there)

## Conclusion
It more than often happens that one gets stuck in simplest of things, gives it hours and reach no where, mostly giving up on the whole situation itself. I realised it's rather better to ask someone or take your mind off from the whole deadlock itself for a while. I am often scared to ask simple questions wondering that I might seem stupid, but the thing is it can save you hours and one might guide you in the right direction when you are running in the exact opposite.

Happy hacking!

