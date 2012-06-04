---
layout: post
title: "Template-based certificate subject generation"
category: core
tags: [core, configuration, template]
---
{% include JB/setup %}

[Mailing list post:](http://permalink.gmane.org/gmane.comp.security.openxpki.devel/90) 


	Alexander Klink | 17 Apr 16:37

	801: Template based certificate subject generation

	Hi all,

	I've just committed 801. The biggest change there is the introduction
	of what I call template-based certificate subject generation, which is
	another feature we now have that the competition does not 
	The general idea is that we don't want to ask the user for the
	certificate subject (or parts thereof), as most users will probably
	not be fluent in terms like CN, DC, etc. But we have a pretty good
	idea or even a formal naming convention, so we know how the certificate
	for a certain use case should look like. One example that has been
	implemented in the default deployment is that a TLS server certificate
	should have a CN of hostname:port, but only if port is not 443.
	This means that we only have to ask the user for the hostname and port
	(optionally) which are things he can relate to.
	In the code, I've taken the concept of the subject styles and
	implemented different styles there. The configuration takes place
	in profile.xml. Let's have a look at an example from the new default
	deployment:

	<profiles id="default_profiles">
	  [...]
	  <endentity>
	    [...]
	    <profile id="I18N_OPENXPKI_PROFILE_TLS_SERVER"
		     super="../profile{default}">
	      <subject id="user_basic_style">
		<label>I18N_OPENXPKI_PROFILE_USER_BASIC_STYLE</label>
		<description>I18N_OPENXPKI_PROFILE_USER_BASIC_DESC</description>
		<template>
		     <!-- default: min=1, max=1 -->
		     <!-- note that the regex is pretty restrictive, one might
			  want to change that to something more liberal -->
		     <input id="username"
			    label="I18N_OPENXPKI_USERNAME"
			    description="I18N_OPENXPKI_USERNAME_DESC"
			    type="freetext"
			    match="\A [A-Za-z]+ \z"
			    width="20"
			    default="testuser"/>
		     <input id="realname"
			    label="I18N_OPENXPKI_REALNAME"
			    description="I18N_OPENXPKI_REALNAME_DESC"
			    type="freetext"
			    match=".+"
			    width="40"
			    default=""/>
		     <input id="email"
			    label="I18N_OPENXPKI_EMAILADDRESS"
			    description="I18N_OPENXPKI_EMAILADDRESS_DESC"
			    type="freetext"
			    match=".+@.+"
			    width="30"
			    default=""/>
		</template>

	As you can see, you can define various input fields in the <template>
	section. Typically, those are of type "freetext", but you can also
	use "select" input fields that have different options. They have a label
	and a description so that the user can figure out what they are for.
	They can be optional (min="0") or you can allow the user to have more
	than one of them (max="..."). And you can match them against regexs for
	input validation, of course.

		<dn>CN=[- realname -]+UID=[- username -],DC=Test Deployment,DC=OpenXPKI,DC=org</dn> 

	This is the Template::Toolkit template that transform the input from the
	fields above into a certificate subject. You can use the complete power
	power of TT here, for example iterating over arrays, conditionals, etc.
	The only thing to note is that the [- ... -] notation is used here, as
	[% ... %] is already used in the deployment step.

		<additional_information>
		</additional_information>

	Additional information can be dependent on the subject style as well.
	Here, you can ask the user for comments, phone numbers, change request
	IDs, etc.

		<subject_alternative_names>
		    <san id="dns">
			<key type="fixed">email</key>
			<value type="fixed">[- email -]</value>
		    </san>
		</subject_alternative_names>

	The subject alternative name can be pre-filled using values from the
	form as well. You can also allow the user to specify additional subject
	alternative names (including specifying their own OIDs), for details see
	the advanced template.

	      </subject>
	      <subject super="../../subject{advanced_style}"/>

	Please test it, I hope that it works now (did I mention that I think the
	web interface code is the ugliest of the whole project?) ...

	Best regards,
	    Alex
