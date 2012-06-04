---
layout: post
title: "Workflow ACLs"
category: core
tags: [core, workflow, acl]
---
{% include JB/setup %}

[Mailing list post:](http://permalink.gmane.org/gmane.comp.security.openxpki.devel/138) 

	Alexander Klink | 22 Jun 11:40

	Workflow ACLs

	Hi all,

	We'll be working on another new feature (bear with us, it's hopefully
	one of the last new features) soon - an enterprise level password safe.
	The idea is to create two new workflow types: one to generate a
	symmetric encryption key which is safed in the workflow, encrypted using
	an assymetric key from a certificate which is configured in the config
	file. This workflow can then encrypt and decrypt (using appropriate
	activities) data for callers from a second type of workflow that is used
	to input and store that data (and retrieve it upon request - one could
	even imagine our standard approval part there, also this is not our
	current goal). As these workflows contain ciphertext that we don't
	necessarily want to give to anyone who has access to the system (as
	do the certificate request workflows with key generation on the CA,
	BTW), we will need an ACL system for viewing workflows, too. This also
	has the added benefit that a user will only see his or her workflows in
	the workflow list.
	Here is a quick proposal for a format that is to be included in the ACL
	(and the standard set of permissions):

	<workflow_permissions role=""> <!-- Anonymous -->
	  <server name="*">
	    <!-- the anonymous user can create and read CSR and CRR workflows
		 as well as SCEP on all servers -->
	    <create>
	       <type>I18N_OPENXPKI_WF_TYPE_CERTIFICATE_SIGNING_REQUEST</type>
	    </create>
	    <create>
	       <type>I18N_OPENXPKI_WF_TYPE_CERTIFICATE_SIGNING_REQUEST_OFFLINE_CA</type>
	    </create>
	    <create>
	       <type>I18N_OPENXPKI_WF_TYPE_CERTIFICATE_REVOCATION_REQUEST</type>
	    </create>
	    <create>
	       <type>I18N_OPENXPKI_WF_TYPE_CERTIFICATE_REVOCATION_REQUEST_OFFLINE_CA</type>
	    </create>
	    <create>
	       <type>I18N_OPENXPKI_WF_TYPE_SCEP_REQUEST</type>
	    </create>
	    <read> <!-- read also means list and execute activity (if the ACL condition holds) -->
	       <creator>$self</creator>
	       <type>I18N_OPENXPKI_WF_TYPE_CERTIFICATE_SIGNING_REQUEST</type>
	    </read>
	    <read>
	       <creator>$self</creator>
	       <type>I18N_OPENXPKI_WF_TYPE_CERTIFICATE_SIGNING_REQUEST_OFFLINE_CA</type>
	    </read>
	    <read>
	       <creator>$self</creator>
	       <type>I18N_OPENXPKI_WF_TYPE_CERTIFICATE_REVOCATION_REQUEST</type>
	    </read>
	    <read>
	       <creator>$self</creator>
	       <type>I18N_OPENXPKI_WF_TYPE_CERTIFICATE_REVOCATION_REQUEST_OFFLINE_CA</type>
	    </read>
	    <read>
	       <creator>$self</creator>
	       <type>I18N_OPENXPKI_WF_TYPE_SCEP_REQUEST</type>
	    </read>
	  </server>
	</workflow_permissions>
	<workflow_permissions role="User">
	  <!-- someone with a 'User' role can do everything the anonymous
	       user can + smartcard personalization -->
	  <server name="*" super="../workflow_permissions{role:}/server{name:*}">
	     <create>
		<type>I18N_OPENXPKI_WF_TYPE_SMARTCARD_PERSONALIZATION</type>
	     </create>
	     <read>
		<creator>$self</creator>
		<type>I18N_OPENXPKI_WF_TYPE_SMARTCARD_PERSONALIZATION</type>
	     </read>
	  </server>
	</workflow_permissions>
	<workflow_permissions role="RA Operator">
	  <server name="*">
	    <create>
	       <type>I18N_OPENXPKI_WF_TYPE_CERTIFICATE_SIGNING_REQUEST</type>
	    </create>
	    <create>
	       <type>I18N_OPENXPKI_WF_TYPE_CERTIFICATE_SIGNING_REQUEST_OFFLINE_CA</type>
	    </create>
	    <create>
	       <type>I18N_OPENXPKI_WF_TYPE_CERTIFICATE_REVOCATION_REQUEST</type>
	    </create>
	    <create>
	       <type>I18N_OPENXPKI_WF_TYPE_CERTIFICATE_REVOCATION_REQUEST_OFFLINE_CA</type>
	    </create>
	    <read> 
	       <creator>.*</creator>
	       <type>I18N_OPENXPKI_WF_TYPE_CERTIFICATE_SIGNING_REQUEST</type>
	    </read>
	    <read>
	       <creator>.*</creator>
	       <type>I18N_OPENXPKI_WF_TYPE_CERTIFICATE_SIGNING_REQUEST_OFFLINE_CA</type>
	    </read>
	    <read>
	       <creator>.*</creator>
	       <type>I18N_OPENXPKI_WF_TYPE_CERTIFICATE_REVOCATION_REQUEST</type>
	    </read>
	    <read>
	       <creator>.*</creator>
	       <type>I18N_OPENXPKI_WF_TYPE_CERTIFICATE_REVOCATION_REQUEST_OFFLINE_CA</type>
	    </read>
	    <read>
	       <creator>.*</creator>
	       <type>I18N_OPENXPKI_WF_TYPE_SCEP_REQUEST</type>
	    </read>
	    <read>
	       <creator>.*</creator>
	       <type>I18N_OPENXPKI_WF_TYPE_SMARTCARD_PERSONALIZATION</type>
	    </read>
	    <read>
	       <creator>.*</creator>
	       <type>I18N_OPENXPKI_WF_TYPE_CRL_ISSUANCE</type>
	    </read>
	    <read>
	       <creator>.*</creator>
	       <type>I18N_OPENXPKI_WF_TYPE_CERTIFICATE_ISSUANCE</type>
	    </read>
	  </server>
	</workflow_permissions>
	<workflow_permissions role="CA Operator">
	  <server name="*" super="../workflow_permissions{role:}/server{name:*}">
	     <!-- the CA operator can do everything the RA Operator can, plus
		  creating CRL and certificate issuance workflows -->
	      <create>
		 <type>I18N_OPENXPKI_WF_TYPE_CRL_ISSUANCE</type>
	      </create>
	      <create>
		 <type>I18N_OPENXPKI_WF_TYPE_CERTIFICATE_ISSUANCE</type>
	      </create>
	  </server>
	</workflow_permissions>

	So we can now limit which role can create which workflows. This is
	currently available in conditions in the initial activity of the
	workflow. I believe it should move into the ACL file, because of the
	easier namespace configuration and because we can then more easily find
	out who can create what and display it on the web interface accordingly
	(no need to show an option to create a CRL issuance workflow if it fails
	on him anyways).
	We can also limit on who can view what kinds of workflows. The proposed
	syntax can "only" limit by type and creator but it could easily be enhanced
	later to also limit by workflow context content for example (this would
	for example allow for the creation of groups of registration officers
	who can only see one part of the CSRs).
	Please let me know what you think on that, if I've forgotten anything
	important or my syntax is messed up ...

	Best regards,
	    Alex
