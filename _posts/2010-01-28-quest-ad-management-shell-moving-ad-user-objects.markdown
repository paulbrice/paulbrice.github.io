---
layout: post
title: Quest AD Management Shell - Moving AD User Objects
date: 2010-01-28 19:00:00 -07:00
categories: ad
---
Moving disable user objects in Active Directory using PowerShell and Quest Management cmdlets.
Moving all disabled users in the '/Users' OU to the '/Users/Disabled' OU.

{% highlight powershell linenos %}
Add-PSSnapin -Name Quest.ActiveRoles.ADManagement -ErrorAction SilentlyContinue
$Users = Get-QADUser -SearchRoot 'ad.domain.com/Users/' -Disabled
Write-Host 'Moving $Users.Count Users."
ForEach($User In $Users){
  Move-QADObject $User.DN -NewParentContainer 'ad.domain.com/Users/Disabled'
}
{% endhighlight %}

Hope this helps

All information is provided on an AS-IS basis, with no warranties and confers no rights.
