---
layout: post
title: Exchange2010 - Rollup Update Version
date: 2012-09-30 20:55:00 -07:00
categories: powershell
---
After recently updating my lab to rollup update 4 for Exchange 2010 Sp2 I realized that the version information being display in the the EMC and EMS was not showing the recent update that I had performed. Luckly as usual there are plenty of articles out there explaining that Exchange 2007 and newer do not reflect updates in either the EMC or EMS.

Solution: You have to look at the ExSetup.exe file metadata for correct update information.
 
Ref: Exchange Team Blog Link: [Dude, Where's My Rollup?][external-link]

So I wrote a quick script to get the information from the ExSetup.exe file from multiple servers.

Run From E.M.S (Exchange Management Shell)

{% highlight powershell linenos%}
$Computers = Get-ExchangeServer | WHERE{$_.ServerRole -ne "None"}
ForEach($Computer In $Computers)
{
$FullPath = "\" + $Computer.Name + "\" + ($Computer.DataPath -Replace "Mailbox$","Bin" -replace ":","$") + "\ExSetup.exe"
$File = Get-ChildItem -Path $FullPath
$ExVer = New-Object PSObject
Add-Member -InputObject $ExVer -MemberType NoteProperty -Name Computer -Value $Computer.Name
Add-Member -InputObject $ExVer -MemberType NoteProperty -Name FileVersion -Value $File.VersionInfo.FileVersion
Add-Member -InputObject $ExVer -MemberType NoteProperty -Name ProductVersion -Value $File.VersionInfo.ProductVersion
$ExAll += $ExVer
}
$ExAll | FT -AutoSize
{% endhighlight %}

Hope this helps.
 
All information is provided on an AS-IS basis, with no warranties and confers no rights.

[external-link]: http://blogs.technet.com/b/exchange/archive/2010/03/08/3409469.aspx
