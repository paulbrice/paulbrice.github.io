---
layout: post
title: Exchange 2010 Certificates - Assigned Services
date: 2012-09-30 20:58:00 -07:00
categories: exchange2010 powershell
---
Here is a PowerShell script to validate your installed server certificates for Exchange 2010 servers and the services that they have been bound to. 

- Mailbox servers you can use -  Get-MailboxServer 
- Transport(HUB) servers you can use -  Get-TransportServer 
- Client Access(CAS) servers you can use -  Get-ClientAccessServer 

The default input for the "Get-ExchangeCertificate" is the "-thumbprint" of the certificate so I had to write this little script.

{% highlight powershell linenos%}
$Servers = Get-MailboxServer 
ForEach($Server In $Servers) 
{ 
$cert = Get-ExchangeCertificate -Server $Server
$cert | Select @{N=&quot;Computer&quot;;E={$Server}},Services,Issuer,Thumbprint 
}
{% endhighlight %}

Hope this helps.
 
All information is provided on an AS-IS basis, with no warranties and confers no rights.

