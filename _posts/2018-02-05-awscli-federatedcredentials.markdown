---
layout: post
title: AWSCLI using Federated Credentials
date: 2018-02-06 19:55:00 -07:00
categories: awscli golang python
---
AWSCLI using Federated Credentials?

A while back i found a blog post written by '[Quint Van Demen][1]' on the AWS Security Blog. This solved a need to leverage federated credentials from our forms based IDP for AWS CLI access and enable us to remove all operational IAM users.

This article was great and using the Python script provided we had a solution. However once this needed to go beyond my team I felt we needed something a little more portable. I looked around and GoLang was interesting as its very easy to port to different OS platforms.

So after Christmas dinner and further helpings of Christmas pudding here is my GoLang port '[awsclifedcred][2]' of the excellent Python script from 'Quint Van Demen'.

Hope this helps

All information is provided on an AS-IS basis, with no warranties and confers no rights.

[1]: https://aws.amazon.com/blogs/security/how-to-implement-a-general-solution-for-federated-apicli-access-using-saml-2-0/
[2]: https://github.com/paulbrice/awsclifedcred
