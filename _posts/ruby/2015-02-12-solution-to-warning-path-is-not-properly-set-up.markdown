---
layout: post
title: "Solution to 'Warning: PATH Is Not Properly Set Up' "
modified:
categories: ruby
description:
tags: [rvm]
image:
  feature: RoR Banner.jpg
  credit:
  creditlink:
comments:
share:
date: 2015-02-12T19:26:33+11:00
---

When I upgrade ruby using ```rvm get stable``` command, the terminal returns me a warning.

The issue in [stackoverflow](http://stackoverflow.com/questions/18276701/getting-warning-path-is-not-properly-set-up-when-doing-rvm-use-2-0-0-defaul) is pretty similiar to mine. I tried to use the top-rated solution posted by Michael Durrant, but it does not work for me. I found another solution of [7stud](https://github.com/wayneeseguin/rvm/issues/2050) that works for me, which is:

>@mpapis That worked. After doing: <br />
>~$rvm get head --auto-dotfiles <br />
>I still got the warning with every rvm command. But after I Quit the Terminal App and relaunched it, the warning went away. Thanks.
