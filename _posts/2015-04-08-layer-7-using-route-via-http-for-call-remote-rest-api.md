---
layout: post
title:  "Layer 7: Using Route via HTTP(S) for call remote rest API"
date:   2015-04-08 17:00:00
categories: layer7
tags: layer7 layer7-policy security
---

>Gateway 8.2, Mobile Access Gateway

Final Result:
===

![25]({{site.baseUrl}}/assets/Using Route via HTTP for call rest API/25.png)

1. Prepare the parameters what will pass to REST API.
2. Call remote REST API.
3. Parse the response from REST API back.
4. Using the response according to requirements.

---

Steps:
===

1\. Drag assertion `set context variable` into your target policy, prepare the parameters what will pass into the remote REST api, it's json data in this example.

![5]({{site.baseUrl}}/assets/Using Route via HTTP for call rest API/5.png)

| **Variable Name** | will using it later|
|**Date Type**|must be message if the variable using for Route via HTTP(S) assertion|
|**Content-Type**|application/json;charset=utf-8 in this example, can change it according the requirements of remote REST API|
|**Experssion**|variable body, can include other variable|

2\. Drag assertion `Route via HTTP(S)` into you target policy, configure URL, HTTP Method, Request Source, Response Destination.

![10]({{site.baseUrl}}/assets/Using Route via HTTP for call rest API/10.png)

|**Request Source**|select the variable of save parameters|
|**Response Destination**|named the varaible for save response, will use in next step|

3\. Drag assertion `Evaluate Regular Expression` for parse the response string from REST API back.

![15]({{site.baseUrl}}/assets/Using Route via HTTP for call rest API/15.png)

|**Sourece: Other Context Variable**|the variable to evaluate the reguler expression configured|
|**Destionation: Context Variable**|the variable to save value of captured group|
|**Include matched substring in capture**|for example, data is `"{"result":"true"}"`, regular expression is `"result":"(	[true|false])"`|
||checked: `destionationVariable[0]="result":"true"`|
||unchecked: `destionationVariable[0]=true`|

4\. Using the response after parse

![20]({{site.baseUrl}}/assets/Using Route via HTTP for call rest API/20.png)


Q: Certificate not verified when access REST api by HTTPS

>com.l7tech.server.policy.assertion.ServerHttpRoutingAssertion: 4042: Problem routing to https://apiserver/service/auth.json. Error msg: Unable to obtain HTTP response from https://apiserver/service/auth.json: Certificate not verified. Caused by: Certificate path validation and/or revocation checking failed


