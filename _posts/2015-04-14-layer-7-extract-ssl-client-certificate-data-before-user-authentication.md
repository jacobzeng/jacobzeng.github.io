---
layout: post
title:  "Layer 7: Extract SSL client certificate data before user authentication"
date:   2015-04-14 17:00:00
categories: layer7
tags: layer7 layer7-policy
---

>Gateway 8.2, Mobile Access Gateway

User Story
===

Sometimes, you want replace client certificate authentication assertions in layer 7 with your own auth server, now you need extract the SSL client certificate data which the auth server need by yourself and sent to the auth server.

---

Steps
===

1\. Drag assertion `Require SSL or TLS Transport` into your target policy
![5]({{site.baseUrl}}/assets/Extract SSL client certificate data before user authentication/5.png)

2\. Double click the assertion, check `Require Client Certificate Authentication`, then ok  
![10]({{site.baseUrl}}/assets/Extract SSL client certificate data before user authentication/10.png)

3\. Access the target url in browser, browser ask you select client certificate for access the target resrouce.
![11]({{site.baseUrl}}/assets/Extract SSL client certificate data before user authentication/11.png)

4\. Now, we can get the certificate data from parameter `${request.ssl.clientCertificate}`
![15]({{site.baseUrl}}/assets/Extract SSL client certificate data before user authentication/15.png)

---

Credential Certificates Variables
===

|**${request.ssl.clientCertificate}**|eturns the client side SSL certificate presented by the requestor (this is an X509Certificate object.)|
|**${request.ssl.clientCertificate.base64}**|Returns the same certificate as above, but as a Base64- encoded string with no white spaces.|
|**${request.ssl.clientCertificate.pem}**|Returns the same certificate as above, but as a PEM- encoded string; this is formatted in Base64 with newlines, enclosed in the following wrapper:<br/>-----BEGIN CERTIFICATE-----<br/> -----END CERTIFICATE-----|

---

Certificate Attributes Variables
===

Please refer to `Layer 7 Policy Manager User Manual, v8.2.pdf` for get more detail about it.
