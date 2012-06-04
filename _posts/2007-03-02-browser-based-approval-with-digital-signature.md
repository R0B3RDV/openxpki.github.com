---
layout: post
title: "Browser-based approval using digital signatures"
category: core
tags: [core, digital signature, browser, approval]
---
{% include JB/setup %}

[Mailing list post:](http://permalink.gmane.org/gmane.comp.security.openxpki.devel/76)


	Alexander Klink | 2 Mar 18:10

	769: Approval with browser-based signatures

	Hi all,

	I've been working on browser-based signature for approval the last
	few days, you can see the result in the latest checkin. Here is
	the round-up on how it works:

	The user (as usual) goes to the approval page for a CSR.
	Here, a new button has appeared which can be used to create a
	signed approval.

	The Mason code retrieves the translated text which is to be
	signed using the new API method get_approval_message. The approval
	message includes the workflow ID, a hash of the CSR data (i.e.
	the PKCS#10 or SPKAC data) and a hash of the current session ID
	(which is basically used as a nonce so that another user can not
	reuse this signature).

	Once the message is signed (using either signText() in Mozilla or
	CAPICOM and SignedData in Internet Explorer, it gets passed on to the approve
	workflow activity, which invokes the ApprovalSignature validator.
	If no signature is defined, the validator checks the signature_required
	configuration option to see whether a signature is needed. If you set
	this option to 1, you can enforce signatures on all CSR approvals.
	If a signature is present, the validator checks that it is valid
	and trusted. For this, I have introduced the trust_anchors option
	in the validator configuration. This is supposed to be a comma-seperated
	list of identifiers. The validator checks whether the certificate
	or a certificate along the chain matches. Thus, you can either define
	end-entity certificates or CA certificates here to do a precise
	match of who you trust to sign an approval.

	Once the validator has validated everything correctly, the
	Approve.pm activity class saves the given approval and all of
	its meta-data (such as the signature, the text that was signed, etc.)
	in the context. It has two new options, too. First of all, defining
	check_creator in the configuration allows to check whether the
	approver is the creator of the workflow and ignores the approval
	in this case. The second setting is multi_role_approvals, with which
	you can specify if you want a user to be able to sign multiple times
	using different roles.

	Once this is done, the Approved.pm condition class checks whether
	enough approvals are present. For this, it prefers the signer_role
	(which is the role of the certificate that was used for signing)
	over the session role, if present.
	I'd be happy if you can start playing with the new code ...

	Regards,
	    Alex

	P.S.: If you want to know why I hate VBScript, read my blog posting
	about the new UnicodeToUTF8 method in Javascript.pm at http://shiftordie.de
