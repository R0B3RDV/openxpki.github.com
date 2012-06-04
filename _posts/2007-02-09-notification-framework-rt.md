---
layout: post
title: "Notification framework and Request Tracker interface"
category: core
tags: [core, notification, rt]
---
{% include JB/setup %}

[Mailing list post:](http://permalink.gmane.org/gmane.comp.security.openxpki.devel/60) 


	Alexander Klink | 9 Feb 19:17

	739: Notification framework and RT

	Hi all,

	I've just checked in the notification framework and the corresponding
	RT notifier. Here is what you need to if you want to try it out:

	- Install RT:
	   - Download it from bestpractical.com/rt/
	   - Install the prerequisites (cpan Bundle::RT)
	   - Configure Apache to use it
	- Configure RT:
	   - Create a new queue for certificate requests (default: Certificate
	     Signing Requests)
	   - Create a new user which is allowed to access this queue 
	- Install RT::Client::REST
	- uncomment the notifier line in Realm 1 from openxpki.conf(.in),
	  redeploy
	- copy the notification directory to your config file directory (this
	  is something that deployment should take care of, if someone could
	  let me know how to do it, I'll implement it there)
	- configure user, password, queue and timeout in notification.xml
	- Login as a user which has an e-mail address as a username
	- Create a CSR and issue a certificate to see the system working
	- Notice that show_instance.html and show_pending_csrs.html link
	  to the ticket system so that the ticket can be easily accessed from
	  within OpenXPKI
	- Adapt notification.xml and the corresponding txt files in the
	  notification directory to your liking, translate them, etc.

	The rest of the post is probably only interesting for the developers,
	as it deals with the implementation.

	So, here is how it works internally:
	- On server initialization, an OpenXPKI::Server::Notification::Dispatcher
	  object is created and added to the context, which is then available as
	  CTX('notification').
	- The constructor of this object looks at the configuration and attaches
	  objects for all configured notifiers.
	- If someone calls CTX('notification')->notify(), the dispatcher sends
	  the notification to all configured notifiers in the current PKI realm.
	  notify() takes a MESSAGE and a WORKFLOW as parameters. The message
	  reflects one configuration in the notification.xml that tells the
	  implementation what actions are done using which template files.
	  The workflow is used so that its variables can be substituted in the
	  templates using Template::Toolkit
	- notify() in the notifier classes is actually implemented in the
	  parent class, OpenXPKI::Server::Notification. This class looks at
	  the configuration to see what actions are to be called, and calls
	  the __do_≤action> methods, that are implemented there as well.
	  They take care of the Template::Toolkit substition and preparing
	  the parameters, so that they can be passed on to the <action> method,
	  which is then implemented in a specific notifier (Notification::RT,
	  for example).

	So much for that. As for the details, just ask or read the source,
	I believe it to be pretty readable.

	Have a nice weekend,
	Regards,
	  Alex