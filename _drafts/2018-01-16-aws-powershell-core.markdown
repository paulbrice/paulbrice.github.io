---
layout: post
title: Using Powershell-Core with AWS
date: 2018-01-16 19:16:00 -07:00
categories: Powershell AWS OSX
---
So after a year and a half, I decided to blow off the cobwebbs and flex the Powershell muscles again. Being a Windows Engineer for 'a few years' and a 'Monad' scripter before Powershell was cool I felt like it was time to what I could do with PS and AWS.

So first using brew I installed [Powershell Core 6.0][external-link0] for OSX

{% highlight bash %}
brew install pwsh
{% endhighlight %}

Then I installed the [AWSPowerShell.NetCore][external-link1] from Powershell Gallery and they have module [documentation][external-link2]. I am using VSCode as my IDE and you can get information on VSCode Powershell plugin [here][external-link3].

{% highlight bash %}
/User/bricep>pwsh
PS /Users/bricep> Install-Module -Name AWSPowerShell.NetCore
{% endhighlight %}

Ok so now I am up and runnig with PSCore, whats next... AWS

{% highlight bash %}
PS /Users/bricep> (get-command -Module AWSPowerShell.NetCore).count
3979
PS /Users/bricep>
{% endhighlight %}

WOW 3979 commands for AWS....

So one of my favorit cmdlets is 'Get-Member', this allows you to see properties and methods on your cmdlets.

Example:

{% highlight powershell %}
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

So as you can see the object returned is a Amazon.EC2.Model.Image listing all properties, methods and alias for the Image class.

Ok a test. Previously I posted about a quick way to find the latest AMI.

Example:

{% highlight bash %}
aws ec2 describe-images --owners amazon \
--filters \
Name=root-device-type,Values=ebs \
Name=architecture,Values=x86_64 \
Name=name,Values=*Windows_Server-2012-R2_RTM-English*Base*

{% endhighlight %}

Lets see what this looks like with Powershell, lets see what commands we have available for AWS AMI Images.

{% highlight powershell %}
PS /Users/bricep> get-command -Name *Image* -Module AWSPowerShell.NetCore

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

So lets use Get-EC2Image.

{% highlight powershell %}
Get-EC2Image --ProfileName default -Region us-east-1 | Get-Member
{% endhighlight %}

[external-link]: https://docs.aws.amazon.com/powershell/latest/userguide/pstools-getting-set-up-linux-mac.html
[external-link0]: https://github.com/PowerShell/PowerShell
[external-link1]: https://www.powershellgallery.com/packages/AWSPowerShell.NetCore/3.3.221.0
[external-link2]: https://docs.aws.amazon.com/powershell/latest/reference/Index.html
[external-link3]: https://github.com/PowerShell/vscode-powershell