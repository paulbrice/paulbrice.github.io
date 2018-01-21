---
layout: post
title: Exchange 2010 - Validating Cluster Services
date: 2012-12-05 02:01:30 -07:00
categories: exchange2010
---
Understanding the state of the cluster service on your Exchange servers is a common need.

There are many ways to acheive this but I do like to create a quick Powershell snipit to speed up my work and share with others.

> ...this is especially handy in DR testing.
 
Note: Run in a "Exchange Management Shell" 

{% highlight powershell linenos %}
Get-MailboxServer | %{GWMI -ComputerName $_.Name -Class Win32_Service | ` 
WHERE{$_.Name -eq &quot;ClusSvc&quot;} | ` 
Select __Server,Name,StartMode,State} 
{% endhighlight %}

This will return all Exchange 2010 servers with a "Mailbox" role and the status of the "Cluster Service".

Hope this helps.
