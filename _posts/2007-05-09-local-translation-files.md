---
layout: post
title: "Support for local changes to translation files"
category: core
tags: [core, i18n, translation, local]
---
{% include JB/setup %}

[Mailing list post:](http://permalink.gmane.org/gmane.comp.security.openxpki.devel/113) 

	Martin Bartosch | 9 May 15:25

	Local i18n translations

	Hi,

	I just checked in some changes to the I18N module that allow people  
	to use their own translations for individual messages.

	If you need local changes for the translations, it is obviously a  
	pain to keep your modified openxpki.po file in sync with the normal  
	commits to the project.

	To address this problem you can now do the following:

	- get a checkout of the trunk/i18n directory
	- add a local translations file, e. g. for the English translation:
	   trunk/i18n/en_GB/openxpki.po.local
	- (do NOT put this file under official version control, track it  
	yourself for your own installation!)

	The build process will detect that a .local file exists and will use  
	its contents in favour of the default values from openxpki.po. You  
	can ADD nonexisting or OVERWRITE existing translations this way in  
	your .local file.

	In order to prevent errors from developers who HAVE a .local file I  
	removed the .mo files from revision control. These files can easily  
	be reconstructed by running

	make scan
	make

	in the checked-out source.

	Package maintainers: please check/update your build scripts for the  
	I18N module.

	Cheers

	Martin
