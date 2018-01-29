---
layout: post
title: Using PowerShell-Core with AWS
date: 2018-01-16 19:16:00 -07:00
categories: PowerShell AWS OSX
---

So after a year and a half, I decided to blow off the cobwebs and flex the PowerShell muscles again. Being a Windows Engineer for 'a few years' and writing scripts in Monad before it became PowerShell. I felt like it was time to see what PowerShell Core was like and play with AWS.

So I used the AWS guide to install [PowerShell Core 6.0][0] for OSX

{% highlight bash %}
brew tap caskroom/cask
brew cask install powershell
brew update
brew cask reinstall powershell
{% endhighlight %}

Then install the [AWSPowerShell.NetCore][2] from PowerShell Gallery and they have module [documentation][3]. I am using VSCode as my IDE and you can get information on VSCode PowerShell plugin [here][4].

{% highlight bash %}
pwsh
Install-Module -Name AWSPowerShell.NetCore
{% endhighlight %}

Another usefull guide. [AWS PowerShell user-guide][1]

Ok so now I am up and running with PSCore, whats next... lets checkout AWS and the available cmdlets.

{% highlight bash %}
(get-command -Module AWSPowerShell.NetCore).count
3979
{% endhighlight %}

WOW 3979 commands for AWS.... lets look at available EC2 Images.

First we need to know what cmdlets are available to work with EC2 Images, we can use Get-Command and filters to find this. PowerShell cmdlet naming standard is based on a Verb/Noun structure which we can filter on using -Verb, -Noun or -Name arguments.

{% highlight PowerShell %}
Get-Command -Name *Image* -Module AWSPowerShell.NetCore

CommandType     Name                                               Version    S
                                                                              o
                                                                              u
                                                                              r
                                                                              c
                                                                              e
-----------     ----                                               -------    -
Cmdlet          Copy-EC2FpgaImage                                  3.3.221.0  A
Cmdlet          Copy-EC2Image                                      3.3.221.0  A
Cmdlet          Edit-EC2FpgaImageAttribute                         3.3.221.0  A
Cmdlet          Edit-EC2ImageAttribute                             3.3.221.0  A
Cmdlet          Get-APSImageBuilderList                            3.3.221.0  A
Cmdlet          Get-APSImageList                                   3.3.221.0  A
Cmdlet          Get-CBCuratedEnvironmentImageList                  3.3.221.0  A
Cmdlet          Get-EC2FpgaImage                                   3.3.221.0  A
Cmdlet          Get-EC2FpgaImageAttribute                          3.3.221.0  A
Cmdlet          Get-EC2Image                                       3.3.221.0  A
Cmdlet          Get-EC2ImageAttribute                              3.3.221.0  A
Cmdlet          Get-EC2ImageByName                                 3.3.221.0  A
Cmdlet          Get-EC2ImportImageTask                             3.3.221.0  A
Cmdlet          Get-ECRImage                                       3.3.221.0  A
Cmdlet          Get-ECRImageBatch                                  3.3.221.0  A
Cmdlet          Get-ECRImageMetadata                               3.3.221.0  A
Cmdlet          Import-EC2Image                                    3.3.221.0  A
Cmdlet          New-APSImageBuilder                                3.3.221.0  A
Cmdlet          New-APSImageBuilderStreamingURL                    3.3.221.0  A
Cmdlet          New-EC2FpgaImage                                   3.3.221.0  A
Cmdlet          New-EC2Image                                       3.3.221.0  A
Cmdlet          Register-EC2Image                                  3.3.221.0  A
Cmdlet          Remove-APSImage                                    3.3.221.0  A
Cmdlet          Remove-APSImageBuilder                             3.3.221.0  A
Cmdlet          Remove-EC2FpgaImage                                3.3.221.0  A
Cmdlet          Remove-ECRImageBatch                               3.3.221.0  A
Cmdlet          Reset-EC2FpgaImageAttribute                        3.3.221.0  A
Cmdlet          Reset-EC2ImageAttribute                            3.3.221.0  A
Cmdlet          Search-REKFacesByImage                             3.3.221.0  A
Cmdlet          Start-APSImageBuilder                              3.3.221.0  A
Cmdlet          Stop-APSImageBuilder                               3.3.221.0  A
Cmdlet          Unregister-EC2Image                                3.3.221.0  A
Cmdlet          Write-ECRImage                                     3.3.221.0  A
{% endhighlight %}

Using **Get-EC2Image** we can pull all the EC2 images available. But we want to filter, so we need to understand the object properties returned for an image.

One of my favorite and invaluable cmdlets is **Get-Member**, this allows you to see properties and methods on your objects.

{% highlight PowerShell %}
Get-EC2Image -Region us-east-1 | Select -first 1 | Get-Member

  TypeName: Amazon.EC2.Model.Image

Name                MemberType    Definition
----                ----------    ----------
BlockDeviceMapping  AliasProperty BlockDeviceMapping = BlockDeviceMappings
ImageState          AliasProperty ImageState = State
Tag                 AliasProperty Tag = Tags
Equals              Method        bool Equals(System.Object obj)
GetHashCode         Method        int GetHashCode()
GetType             Method        type GetType()
ToString            Method        string ToString()
Architecture        Property      Amazon.EC2.ArchitectureValues Architecture {get;set;}
BlockDeviceMappings Property      System.Collections.Generic.List[Amazon.EC2.Model.BlockDeviceMapping] BlockDeviceMappings {get;set;}
CreationDate        Property      string CreationDate {get;set;}
Description         Property      string Description {get;set;}
EnaSupport          Property      bool EnaSupport {get;set;}
Hypervisor          Property      Amazon.EC2.HypervisorType Hypervisor {get;set;}
ImageId             Property      string ImageId {get;set;}
ImageLocation       Property      string ImageLocation {get;set;}
ImageOwnerAlias     Property      string ImageOwnerAlias {get;set;}
ImageType           Property      Amazon.EC2.ImageTypeValues ImageType {get;set;}
KernelId            Property      string KernelId {get;set;}
Name                Property      string Name {get;set;}
OwnerId             Property      string OwnerId {get;set;}
Platform            Property      Amazon.EC2.PlatformValues Platform {get;set;}
ProductCodes        Property      System.Collections.Generic.List[Amazon.EC2.Model.ProductCode] ProductCodes {get;set;}
Public              Property      bool Public {get;set;}
RamdiskId           Property      string RamdiskId {get;set;}
RootDeviceName      Property      string RootDeviceName {get;set;}
RootDeviceType      Property      Amazon.EC2.DeviceType RootDeviceType {get;set;}
SriovNetSupport     Property      string SriovNetSupport {get;set;}
State               Property      Amazon.EC2.ImageState State {get;set;}
StateReason         Property      Amazon.EC2.Model.StateReason StateReason {get;set;}
Tags                Property      System.Collections.Generic.List[Amazon.EC2.Model.Tag] Tags {get;set;}
VirtualizationType  Property      Amazon.EC2.VirtualizationType VirtualizationType {get;set;}
{% endhighlight %}

As you can see from the output above, the object returned is a Amazon.EC2.Model.Image object and all properties, methods and aliases for the object are listed.

Using that information we can construct a PowerShell script, there are a couple ways to filter data in PowerShell.

1. -Filter{} parameter on a cmdlets itself.
2. Where-Object{} cmdlet, allowing pipeline filtering on returned data objects.

Both of these have their place but lets look at the performance when pulling large data sets from AWS.

**Where-Object{} Cmdlet**
{% highlight PowerShell %}
Measure-Command{Get-EC2Image -Region us-east-1 | Where-Object{$PSItem.Platform -eq "windows"} | Select-Object Name}

Days              : 0
Hours             : 0
Minutes           : 1
Seconds           : 20
Milliseconds      : 327
Ticks             : 803275570
TotalDays         : 0.000929717094907407
TotalHours        : 0.0223132102777778
TotalMinutes      : 1.33879261666667
TotalSeconds      : 80.327557
TotalMilliseconds : 80327.557
{% endhighlight %}

**-Filter{} Property**
{% highlight PowerShell %}
$platform_values = New-Object 'collections.generic.list[string]'
$platform_values.add("windows")
$filter_platform = New-Object Amazon.EC2.Model.Filter -Property @{Name = "platform"; Values = $platform_values}
Measure-Command{Get-EC2Image -Owner amazon -Filter $filter_platform -Region us-east-1 | Select name}

Days              : 0
Hours             : 0
Minutes           : 0
Seconds           : 34
Milliseconds      : 766
Ticks             : 347665610
TotalDays         : 0.000402390752314815
TotalHours        : 0.00965737805555555
TotalMinutes      : 0.579442683333333
TotalSeconds      : 34.766561
TotalMilliseconds : 34766.561
{% endhighlight %}

So for this case using the -Filter{} parameter is much faster than filtering the returned data with Where-Object.

To perform filtering using the -Filter{} parameter you need to submit a hash for the specific attribute you want to filter on. If you want to filter on multiple attributes then you create multiple filter objects or hash tables for each filter and wrap them in an array separated by semi-colon. Check the specific cmdlets documentation for the available filters [here][7].

Filters for **Get-EC2Image** [here][6]

I want to filter on both platform, root-device-type and name.

Final Script:

{% highlight PowerShell %}
$platform_values = New-Object 'collections.generic.list[string]'
$rootdevicetype_values = New-Object 'collections.generic.list[string]'
$name_values = New-Object 'collections.generic.list[string]'
$platform_values.add("windows")
$rootdevicetype_values.add("ebs")
$name_values.add("*Windows_Server-2012-R2_RTM-English*Base*")
$filter_platform = New-Object Amazon.EC2.Model.Filter -Property @{Name = "platform";Values = $platform_values}
$filter_rootdevicetype = New-Object Amazon.EC2.Model.Filter -Property @{Name = "root-device-type";Values = $rootdevicetype_values}
$filter_name = New-Object Amazon.EC2.Model.Filter -Property @{Name = "name";Values = $name_values}
$properties = @( $filter_platform ; $filter_rootdevicetype ; $filter_name)
Get-EC2Image -Owner amazon -Filter $properties -Region us-east-1 | Select name
{% endhighlight %}

Here are some more useful links.

- AWS PowerShell [user-guide][5]
- AWS PowerShell [cmdlet-reference][7]

Hope it helps.

All information is provided on an AS-IS basis, with no warranties and confers no rights

[1]: https://github.com/PowerShell/PowerShell/blob/master/docs/installation/macos.md
[2]: https://www.PowerShellgallery.com/packages/AWSPowerShell.NetCore/3.3.221.0
[3]: https://docs.aws.amazon.com/powershell/latest/reference/Index.html
[4]: https://github.com/PowerShell/vscode-PowerShell
[5]: https://docs.aws.amazon.com/powershell/latest/userguide/aws-pst-ug.pdf
[6]: https://docs.aws.amazon.com/powershell/latest/reference/items/Get-EC2Image.html
[7]: https://docs.aws.amazon.com/powershell/latest/reference/
