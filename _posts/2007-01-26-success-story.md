---
layout: post
title: "First successful production deployment of OpenXPKI"
category: success stories
tags: [success story, smartcard]
---
{% include JB/setup %}


<p>
The first production deployment of OpenXPKI was performed on Friday,
2007-01-26 by <a href="http://www.cynops.de/">Cynops GmbH</a> for
a customer.
</p>
<p>
In the current implementation phase OpenXPKI is solely used for  
one single purpose - it implements a self-service application for  
SmartCard personalization.
</p>

<h4>System environment</h4>
<p>
SuSE Linux SLES 8, Oracle 9, nCipher nC1002W/nC3022W/nC4032W HSM,  
Apache 1.3, RSA Access Manager, RSA SID-800 tokens
</p>

<h4>Authentication</h4>
<p>
For user authentication a RSA token based Web 
Single-Sign-On solution implemented with RSA ClearTrust (now called RSA  
Access Manager) is used. The web server configuration looks very much like a  
basic authentication in front of the web application, and it also  
sets some environment variables that the application can evaluate to  
obtain login user information.
</p>

<h4>Authorization</h4>
<p>
The OpenXPKI authorization configuration for users is using an  
"External Static" that only calls /bin/true and sets the role "User".  
(The actual authentication is performed by the authentication module  
configured in the web server config.)
</p>

<p>
RA and CA operator authentication also works via the SSO mechanism.  
However, the logged in user can pick "RA Operator" or "CA Operator"  
instead of "User". The configuration is again "External Static" but  
in this case it calls a shell script which checks the authenticated  
user name against LDAP groups that list acceptable RA/CA operators.
If authorized, the user gets the corresponding role for the rest of  
the session.
</p>

<h4>SmartCard personalization</h4>
<p>
The application uses the SmardCard Personalization workflow  
as shipped in rev 709.
A User can either login normally to the application and pick  
"Personalize SmartCard" from the top level menu. However, using  
mod_rewrite a rewrite rule for https://servername/token was
configured to a deep link into the application. It directly starts the  
personalization workflow and hides the user menu.
</p>
<p>
The workflow queries user data from an LDAP directory and stores the  
required fields in the workflow instance context.
The user is then prompted to insert a SmartCard token. If the correct  
Crypto Service Provider is installed (and the user is using MS  
IE...), a key pair is generated on the token. The browser sends the  
CSR to OpenXPKI which inserts the CSR into the workflow.
</p>
<p>
The HSM-protected CA key is usually always online, so certificate  
issuance can happen right away. The personalization workflow forks a  
certificate issuance workflow and waits for its completion. Once the  
certificate is issued the workflow continues and instructs IE to  
install the certificate on the user's token.
</p>
<p>
In this installations two certificates per user are created, and both are  
requested and installed in the same session. Due to the low speed of  
the used SmartCard tokens the full personalization process takes  
about 4 minutes.
</p>
