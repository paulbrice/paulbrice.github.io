---
layout: post
title: Mailbox Limits in Exchange2003 Sp2
date: 2010-11-04 14:23:00 -07:00
categories: ad exchange2003
---
In Exchange 2003 not having all those excellent cmdlets can be frustrating. I have listed a couple examples here of how to get mailbox limit information using the Powershell, "QUEST AD Management Shell" cmdlets to gather mailbox limit information from AD user objects. 

Get User Mailbox Limit Information from an OU. 

{% highlight powershell linenos%}
Get-QADUser -SearchRoot 'domain.com/users/' -includedProperties "mDBOverHardQuotaLimit","mDBOverQuotaLimit","mDBStorageQuota" | Select Name,Mail,@{N="Warning";E={$_.mDBStorageQuota}},@{N="HardLimit";E={$_.mDBOverQuotaLimit}} 
Get User Mailbox Information from an Text.txt file. 
{% endhighlight %}

{% highlight powershell linenos%}
Get-Content C:\Users.txt | %{Get-QADUser $_ -includedProperties "mDBOverHardQuotaLimit","mDBOverQuotaLimit","mDBStorageQuota" | Select Name,Mail,@{N="Warning";E={$_.mDBStorageQuota}},@{N="HardLimit";E={$_.mDBOverQuotaLimit}}
{% endhighlight %}

Hope this helps. 

All information is provided on an AS-IS basis, with no warranties and confers no rights. 
