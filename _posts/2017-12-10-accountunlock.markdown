---
layout: post
title:  "my AD account is locked!"
date:   2015-8-10 22:40:33 -0700
categories: powershell
---
AD account on timed lock?

The other day I was waiting for my account to unlock, so I had a few minutes to kill so I decided to write a tool to pass the time.

I wrote a script to tell me how long I had left to wait… pretty crude but I had fun.

{% highlight powershell linenos %}
Function Get-LockoutRemainingTime
{
  Param($ID)
  Try
  {
    $Account = Get-ADUser $ID -Properties LockedOut,AccountLockoutTime
    $Domain = [ADSI]"WinNT://$env:userdomain"
    $UnlockInt = $Domain.AutoUnlockInterval
    If($Account.LockedOut -eq $TRUE)
    {
      Write-Host -BackgroundColor DarkRed "Account $($Account.Name) is locked out!"
      [Int32]$i = ($Account.AccountLockoutTime.AddSeconds(1800) | %{New-TimeSpan $(Get-Date) $_}).TotalSeconds
      Do{Write-Host "Account Unlocked in $i seconds";$i--;start-sleep -s 1}While($i -ne 0)
    }
    Else
    {
      Write-Host -BackgroundColor DarkGreen "Account $($Account.Name) is not locked out!"
    }
  }
  Catch
  {
    Write-Host "Script Failure - $($_.Exception.Message)"
  }
}#End Function
 
Get-LockoutRemainingTime -ID "CN or DN"
{% endhighlight %}

Hope this helps.

All information is provided on an AS-IS basis, with no warranties and confers no rights.