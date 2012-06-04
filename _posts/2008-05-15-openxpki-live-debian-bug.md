---
layout: post
title: "OpenXPKI Live (beta) affected by Debian OpenSSL bug"
category: security
tags: [security, bug, live cd, debian, openssl]
---
{% include JB/setup %}

[Mailing list post:](http://permalink.gmane.org/gmane.comp.security.openxpki.devel/239) 

    Alexander Klink | 15 May 23:43
     
    Refrain from using the live CD for anything important!

    Hi,

    I assume most of you have read the Debian advisory regarding
    Debian's own openssl (see http://www.debian.org/security/2008/dsa-1571).
    This also means that our live CD is affected by that, so I thought
    I should probably warn you to not use it for anything except for
    testing - the generated (CA) certificates are not worth the cost
    of the few bytes on the disk they are written to.
    I'll update the live CD soonish, but it will probably take two more
    weeks or so as I'll be in London next week.

    Cheers,
      Alex
