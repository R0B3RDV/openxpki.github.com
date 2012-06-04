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

<p>
  Unlike many other OpenSource PKI projects OpenXPKI offers
  <a href="/features/index.html">powerful features</a>
  necessary for professional environments that are usually
  only found in commercial grade PKI products. (If you have ever wondered
  what could be done to provide <b>continuous operation</b> of a PKI without
  having to struggle with the system every time your CA certificate expires,
  OpenXPKI is probably the right thing for you.)
</p>
<p>
  However, we also target small scale installations by providing 
  quick-start configuration examples that allow to get a usable PKI 
  running quickly.
</p>
<p>
  OpenXPKI runs on most Unix-like operating system (verified on 
  FreeBSD, Linux, Solaris/OpenSolaris and Mac OS X).<br/>
  Database backends exist for MySQL, PostgreSQL, Oracle and DB2.<br/>
  OpenXPKI also integrates with the 
  <a href="http://bestpractical.com/rt/">RT Request Tracker</a> and
  supports nCipher's nShield Hardware Security Modules.
</p>

<dl>
<dt>Architecture White Paper available</dt>
<dd>The OpenXPKI Team has compiled a White Paper on the architecture 
  and key features of the OpenXPKI software. The paper is 
  <a href="docs/OpenXPKI-Architecture-Overview.pdf">available 
    as a PDF Document here</a> and outlines the architecture of the
  system. Development follows the concepts described there closely.
</dd>
</dl>


# News #

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>
[News archive](archive.html)


<dt>2005-Oct-17 to 2005-Oct-18
</dt>
<dd>
<a href="../docs/ws20051018.html">
      2. OpenCA Workshop
</a>
</dd>

<dt>2005-Feb-22</dt>
<dd>
<a href="../docs/dfn_bt_2005_02_22.pdf">
      Core Design
</a>
            presented on a meeting of Germany's National Research and Education Network
</dd>
<dt>2004-Oct-11 to 2004-Oct-12
</dt>
<dd>
<a href="../docs/ws20041012.html">
	      1. OpenCA Workshop
</a>
</dd>

<h2>Security Advisories</h2>
<p>
<a href="../secadvs/index.html">
	    Security advisories</a> are listed in a dedicated section,
	  in order to make it possible to publish updated advisories 
	  there as well.
</p>
<h2>Releases</h2>
<p>OpenXPKI is currently under development. 
	  There are no official releases yet.
</p>



## To-Do

This site is still unfinished. If you'd like to be added as a contributor,
[please fork](http://github.com/openxpki/openxpki.github.com)!

We need to migrate content from the old website and clean up the theme. 


