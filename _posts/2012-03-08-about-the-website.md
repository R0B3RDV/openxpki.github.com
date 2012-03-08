---
layout: post
title: "About the website"
category: website
tags: [website, git]
---
{% include JB/setup %}

# About the website #

With the move from Subversion to Git, there is also a migration to a new
way of managing the content of our website. Using Jekyll with GitHub had
already caught our attention in the past, and the move from svn was the
last nudge needed.

A couple of the advantages of Jekyll, and in our case Jekyll-Bootstrap, are:

* Simple text layout of the files using Markdown
* Focus on content and not layout
* Support for Git and offline viewing
* Ease of deploying updates (just "git push" and the pages are automatically
  updated)

For those interested in creating content, just clone the
[openxpki.github.com](https://github.com/openxpki/openxpki.github.com)
repository on github and take a look at the
[Jekyll Quick Start](http://jekyllbootstrap.com/usage/jekyll-quick-start.html)
for a couple of tips and tricks.

One important note for contributors: in contrast to the code repository, where
the release process will be a bit more formal, pushing commits to the website
repository will be more straightforward. All registered developers will have 
push priviledges and should push to the "master" branch. If you'd like someone
to review your changes before publishing, just fork the repo on github and send
a pull request.

In the upcoming weeks, we will be working on the final switchover from the
current site and the new content.

