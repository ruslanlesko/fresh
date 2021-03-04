---
title: "Let's Encrypt introduces new certificates"
date: 2020-09-21T21:05:49+03:00
description: "Next generation encryption comes to one of the largest certificate issuers."
tags: [Web]
---
[Let's Encrypt's New Root and Intermediate Certificates](https://letsencrypt.org/2020/09/17/new-root-and-intermediates.html)

We don't hear updates on encryption technologies very often, but recently Let's Encrypt, a major certificate authority (CA), has introduced new types of certificates. These certificates use ECDSA encryption algorithm instead of RSA. The main difference is size. RSA public key is about 256 bytes long when ECDSA P-384 public key is only about 48 bytes preserving the same level of security. 

Let's Encrypt is provides free certificates with good tools for their refresh, that is why it is so popular. And this blog is also protected by certificates issued by Let's Encrypt (at the time this article was written). By upgrading to ECDSA Let's Encrypt will be able to instantly upgrade lots of websites and services at once to the new standard. 

