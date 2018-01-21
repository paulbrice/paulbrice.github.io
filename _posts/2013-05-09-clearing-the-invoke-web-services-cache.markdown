---
layout: post
title: Clearing "Invoke Web Services" Cache
date: 2013-05-09 04:23:41 -07:00
categories: scorch
---
 I have had problems when working with a custom WSDL, when we change the WSDL Orchestrator "Invoke Web Services" activity does not see the changes. I have come across this multiple times so I thought I would document it. (for my memory if nothing else)
 Thankfully another person has already solved this, in this [blog post][external-blog].
 
 Orchestrator is caching the data returned by the WSDL in;

 C:\ProgramData\Microsoft System Center 2012\Orchestrator\Activities\WebServices2\
 
 So after reading the above article, I performed these steps. 

- Close Runbook Designer
- Delete those newly created DLLs
- Re-Open 'Runbook Designer'

 This worked for me, feel free to post replies about other experiences or solutions as this is a pain. 
 
 Hope this helps. 
 
 All information is provided on an AS-IS basis, with no warranties and confers no rights

 [external-blog]: http://sc.scomurr.com/orchestrator-2012-sp1-clear-the-cache-for-invoke-web-services/
