---
layout: post
title: Microsoft SMA - Exporting Samples
date: 2015-12-14 22:05:00.000000000 -07:00
categories: powershell sma
---
Running an SMA instance without the 'Azure Pack' makes using SMA is a bit more, interesting!

After looking around i saw that there were several "Sample-" runbooks already built for us in SMA, most of these focused on integrations with other systems.

Runbook Names

- Sample-Using-VMCloud-Automation
- Sample-Using-Activities
- Sample-Managing-WebSiteCloud
- Sample-Using-Variables
- Sample-Managing-Orchestrator
- Sample-Managing-VirtualMachineManager
- Sample-Using-Credentials
- Sample-Using-SuspendWorkflow
- Sample-Managing-MySqlServers
- Sample-Managing-ConfigurationManager
- Sample-Using-Modules
- Sample-Using-Connections
- Sample-Using-Runbooks
- Sample-Managing-OperationsManager
- Sample-Managing-Plans
- Sample-Managing-UserAccounts
- Sample-Managing-Azure
- Sample-Deleting-VMCloud-Subscription
- Sample-Modify-VMCloud-Subscription
- Sample-Using-Checkpoints
- Sample-Managing-VMClouds
- Sample-Using-RunbookParameters
- Sample-Managing-ServiceBusCloud
- Sample-Managing-DataProtectionManager

I am looking at doing some integration in 'SMA' to 'Configuration Manager 2012' and I noticed there were sample Runbooks for 'Configuration Manager'. Now without the Azure Pack' viewing these Runbooks was a little more difecult, as you have to understand the states a Runbook can be in (Published, Draft etc..) to be able to view it.
I wrote this quick script to export the "Sample-" Runbooks for better viewing.

Note: the destination directory must exist. D:\SMA\Runbooks\Samples\

{% highlight powershell linenos %}
$parameters = @{'WebServiceEndpoint' = 'https://server.domain.com','Port' = 9090}
$Runbooks = Get-SmaRunbook @parameters | Where{$_.RunbookName -like '*Sample-*'} | Select RunbookName
$Runbooks | %{$scriptPath = &quot;D:\SMA\Runbooks\Samples\$($_.RunbookName).ps1&quot;; Out-File -InputObject (Get-SMARunbookDefinition @parameters -Name $($_.RunbookName) -Type Draft).Content -FilePath $scriptPath -Force}
{% endhighlight %}


Hope this helps.
 
All information is provided on an AS-IS basis, with no warranties and confers no rights.
