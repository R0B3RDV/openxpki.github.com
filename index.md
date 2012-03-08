---
layout: page
title: OpenXPKI
tagline: An open, enterprise-grade PKI/Trustcenter
---
{% include JB/setup %}

# About OpenXPKI #

The OpenXPKI Project has created an enterprise-grade PKI/Trustcenter software
that supports well-established infrastructure components like RDBMS and 
Hardware Security Modules. Flexibility and modularity are the project's key
design objectives.

# News #

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

## To-Do

This site is still unfinished. If you'd like to be added as a contributor,
[please fork](http://github.com/openxpki/openxpki.github.com)!

We need to migrate content from the old website and clean up the theme. 


