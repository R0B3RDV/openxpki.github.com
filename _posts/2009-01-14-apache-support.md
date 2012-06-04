---
layout: post
title: "Improved Apache Support"
category: frontend
tags: [apache, mod_perl]
---
{% include JB/setup %}

OpenXPKI now works with all versions of Apache at all supported 
platforms in both mod_perl and cgi modes.

[Mailing list post:](http://permalink.gmane.org/gmane.comp.security.openxpki.devel/273) 

	Sergei Vyshenski | 14 Jan 18:30


	i18n in Apache22+ and mod_perl2 for *BSD systems

	Hi,

	Rev. 1389 seems to fix long standing issue with i18n in Apache22+ and
	mod_perl2 for *BSD systems.

	The idea is to move from xs gettext into perl gettext (_pp) modules.
	Note that gettext_pp does not have bind_textdomain_filter function.
	Explicitly used Encode::_utf8_on instead.

	Make notice: Now it is mandatory to set proper variable:
	SetEnv OPENXPKI_LOCALE_PREFIX /usr/local/share/locale
	in your Apache httpd.conf file.
	gettext_pp is too pedantic about this variable.

	Please try if this does not break work of your systems. At the moment
	we here have checked only two lines in the following table, and it
	works fine.

	===============================================================
	Apache ver.          Modperl ver         CGI        System

	2.2.11                  2.0.4             -          FreeBSD-7.1

	1.3.41                  1.30              -          FreeBSD-7.1
	===============================================================

	All the best, Sergei
