---
layout: post
title:  "Java Exception: sun.security.validator.ValidatorException"
date:   2015-03-20 22:05:40
categories: exceptions
tags: exceptions java
---

>javax.net.ssl.SSLHandshakeException: sun.security.validator.ValidatorException: PKIX path building failed: sun.security.provider.certpath.SunCertPathBuilderException: unable to find valid certification path to requested target

Solution 1
------
This issue occurs when a server changes their HTTPS SSL certificate, and our older version of java doesn’t recognize the root certificate authority (CA).

If you can access the HTTPS URL in your browser then it is possible to update Java to recognize the root CA.

Gathered the certificate information
======

1. In your browser, go to the HTTPS URL that Java could not access. Click on the HTTPS certificate chain (there is lock icon in the Internet Explorer), click on the lock to view the certificate.

2. Go to “Details” of the certificate and “Copy to file”. Copy it in Base64 (.cer) format. It will be saved on your Desktop.

Now  had to make the current using java version to know about the certificate so that further it doesn’t refuse to recognize the URL. The root certificate information stays by default in JDK’s `\jre\lib\security` location, and the default password to access is: `changeit`.

	"%JRE_HOME%/bin/keytool" –import –noprompt –trustcacerts –alias ALIASNAME -file FILENAME_OF_THE_INSTALLED_CERTIFICATE -keystore "%JRE_HOME%/lib/security/cacerts" -storepass changeit

if using java7 or above
	
	"%JRE_HOME%/bin/keytool" -importcert -trustcacerts -alias ALIASNAME -file FILENAME_OF_THE_INSTALLED_CERTIFICATE -keystore "%JRE_HOME%/lib/security/cacerts" -storepass changeit

Solution 2
------

Fake the trust manager

	TrustManager[] trustAllCerts = new TrustManager[] { new X509TrustManager() {
                public java.security.cert.X509Certificate[] getAcceptedIssuers() {
                    return null;
                }
                
                public void checkClientTrusted(X509Certificate[] certs,
                                               String authType) {
                }
                
                public void checkServerTrusted(X509Certificate[] certs,
                                               String authType) {
                }
                
            } };

	context = SSLContext.getInstance("SSL");
	context.init(keyManagerFactory.getKeyManagers(), trustAllCerts,new SecureRandom());