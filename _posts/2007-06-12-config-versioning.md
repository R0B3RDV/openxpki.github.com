---
layout: post
title: "Configuration versioning"
category: core
tags: [core, configuration, versioning]
---
{% include JB/setup %}

[Mailing list post:](http://permalink.gmane.org/gmane.comp.security.openxpki.devel/127) 

	Alexander Klink | 12 Jun 17:28

	Configuration versioning

	Hi all,

	I'll be committing a rather large set of changes in a second. Here is
	the short version on what I've been working on for the last 4 weeks
	(the detailed version with all steps in between can be seen in the
	config_versioning branch in my Git repository). I'll call the feature
	"Configuration Rollover" 

	It started with feature requested #1637681 (Use our own configuration
	files for workflow). While thinking about how to implement it best
	(adding the configuration using Workflow::Factory's add_config()
	method), Martin and I discussed that this is a good point in time to
	address the workflow versioning feature request (#1606727), too.
	A few minutes later, we realized that just having a version management
	for the workflow's own configuration was not enough - workflows also
	use "global" configuration all over the place (for example certificate
	profiles, etc.). So we decided that it would be useful if we could just
	have different configuration versions in the database that can then
	be used by the workflows that need them. Thus, ultimately, workflows
	can run with the exact configuration with which they started without
	having to deal with any configuration changes introduced in the
	meantime.

	This has been implemented in the following way:
	- During server initialization, the server reads the configuration that
	  is currently available on disc. If it is not yet part of the CONFIG
	  table in the database, it is added to the database. It then proceeds
	  to instantiate a XML::Config object that gets passed the serialized
	  versions of the config caches. XML::Config now no longer uses just
	  one XML::Cache object, but one for each configuration. To distinguish
	  the configurations, they have a config ID, which is just the SHA1
	  hash of the serialization.
	- As lots of configuration has been moved to CTX('pki_realm') for easier
	  access, I have also introduced CTX('pki_realm_by_cfg'), which creates
	  the structure for each configuration and is indexed by config ID (thus
	  CTX('pki_realm_by_cfg')->{$current_config_id} == CTX('pki_realm')
	  holds true).
	- For the workflow configuration, we instantiate a workflow factory
	  for every configuration in the database (which is then used when
	  instantiating the corresponding workflow). Workflows have a context
	  entry config_id, which references the configuration which was present
	  when they were started. To do so, we had to use our own workflow
	  factory, because Workflow::Factory is a singleton only. This
	  introduces the problem that Workflow.pm says you can subclass it,
	  but it is not really true (for more details see
	  http://rt.cpan.org/Public/Bug/Display.html?id=18159). This means
	  there are some more evil hacks regarding the symbol table in the
	  code until Workflow has been patched to support real subclassing.
	- To enforce that workflows don't use the current configuration instead
	  of their own by accident, the caller is checked when calling
	  get_xpath_* and we throw an exception if the caller is in the Workflow
	  namespace and does not provide a CONFIG_ID parameter. This also means
	  that some core methods had to be modified to support the passing of
	  CONFIG_ID, because they call get_xpath() respectively. A similar
	  method is used to prohibit the direct access of CTX('pki_realm') from
	  within workflow classes.

	The tests run fine here(tm), let's cross your fingers for me that they work
	on the automated testing machine ...

	Also, I'd appreciate some manual testing - I've tested some things for
	which we do not have automated tests, such as LDAP publication (although
	I believe it should be possible to address that using Net::Server::LDAP)
	but I just might have missed something ...

	Best regards,
	  Alex
