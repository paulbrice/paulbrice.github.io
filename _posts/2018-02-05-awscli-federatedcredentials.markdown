---
layout: post
title: AWSCLI using Federated Credentials
date: 2010-01-28 19:00:00 -07:00
categories: ad
---
AWSCLI using Federated credentials?

A while back i found a blog post written by 'Quint Van Demen' on the AWS Security Blog, solving an issue I had in my job where we would like to leverage federated credentials for AWS CLI access and remove IAM users. This article was great and uisng the Python script provided we had a solution. However once this needed to go beyound my team I felt we needed something a little more portable. I looked around and GOLang was interesting as its very easy to port to different OS platforms.

So here is my GoLang port '[awsclifedcred][1]' of the excellent Python script from 'Quint Van Demen'.

Hope this helps

All information is provided on an AS-IS basis, with no warranties and confers no rights.


[1]: https://github.com/paulbrice/awsclifedcred
