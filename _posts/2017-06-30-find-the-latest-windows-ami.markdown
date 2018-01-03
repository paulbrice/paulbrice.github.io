---
layout: post
title: AWS - Get the latest Windows AMI's
date: 2017-06-30 20:00:34 -07:00
categories: aws
---
The fastest way to get a list of available AMI's in AWS is to use the AWS CLI.
If you have not used it yet go [here][aws-cli] and learn it!

For this we use the 'ec2 describe-images' base command;

{% highlight bash linenos %}
aws ec2 describe-images --owners amazon \
--filters \
Name=root-device-type,Values=ebs \
Name=architecture,Values=x86_64 \
Name=name,Values=*Windows_Server-2012-R2_RTM-English*Base*
{% endhighlight %}

That gets us is a huge list of all Windows 2012 R2 AMI's.
What we need to do is filter, that is easy using JMESPath.
...lets filter the return by selecting just the Name, ImageId and CreatedDate.

{% highlight bash linenos %}
aws ec2 describe-images --owners amazon \
--filters \
Name=root-device-type,Values=ebs \
Name=architecture,Values=x86_64 \
Name=name,Values=*Windows_Server-2012-R2_RTM-English*Base* \
--query 'Images[].{ID:ImageId,Name:Name,Created:CreationDate}'
{% endhighlight %}

So better but still a lot of AMI's to look through.
Lets filter by CreationDate, get all AMI's created after May-01 2017.

{% highlight bash linenos %}
aws ec2 describe-images --owners amazon \
--filters \
Name=root-device-type,Values=ebs \
Name=architecture,Values=x86_64 \
Name=name,Values=*Windows_Server-2012-R2_RTM-English*Base*

{% endhighlight %}
So now we have a better list!
As you can see AWS CLI and JMESPath for the win!

Hope this helps.
 
All information is provided on an AS-IS basis, with no warranties and confers no rights.

[aws-cli]: https://aws.amazon.com/documentation/cli/
