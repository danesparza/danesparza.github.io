---
date: "2012-01-09"
url: /2012/01/using-wildcards-with-git-operations
title: "Using wildcards with git operations"
---
I've been using the awesome '<a href="http://git-scm.com/" target="_blank">git</a>' source code control system for the past year now.  The transition from Subversion to git was prompted mostly by my desire to use the awesome application hosting platform <a href="http://www.appharbor.com" title="AppHarbor" target="_blank">AppHarbor</a>, but I picked up git with ease and haven't looked back. 

One of the things I've learned about using git in conjunction with Visual Studio 2010 is that <a href="http://stackoverflow.com/a/507369/19020" target="_blank">git usually likes to be in control of certain operations</a>.  In particular, git likes to be in control of 'remove' operations.  

Removing a file from git is as simple as running 

	rm filepath/filename.ext

from the git command line, but when that is a long path and filename, it can get a bit ... tedious.

Luckily, there are a few quick tips I can share when using the git commandline:  

<strong>First</strong>, the tab key provides command-line completion, just like in a Linux 'bash' prompt.

<strong>Second </strong>(and perhaps even more awesome) -- you can use bash (or even DOS-style) wildcards for the filename!  

This means that you can easily use the following syntax:

	rm MyApp.Library/Some/Really/Long/Path/MyLongFile*.*

How cool is that!?

Site note: If you're learning git (or just want a nice desk companion for Git) you can't go wrong with this book:

<a href="http://www.amazon.com/gp/product/1430218339/ref=as_li_tl?ie=UTF8&camp=1789&creative=390957&creativeASIN=1430218339&linkCode=as2&tag=theblogofdane-20&linkId=R4IUVS7PYYH735KN"><img border="0" src="/img/pro_git.jpg" ><br/>Pro Git</a><img src="http://ir-na.amazon-adsystem.com/e/ir?t=theblogofdane-20&l=as2&o=1&a=1430218339" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />


