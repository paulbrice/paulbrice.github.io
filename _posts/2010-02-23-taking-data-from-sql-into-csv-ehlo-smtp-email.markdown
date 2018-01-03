---
layout: post
title: SQL Export [INTO] CSV
date: 2010-02-23 22:48:00 -07:00
categories: powershell sql smtp
---
Today I was looking at producing a report from a SQL2005 database and exporting the SQL data into a CSV file. Once completed I would be sending the CSV file to a member of my team via Email. As this is a common request I wanted to automate this with PowerShell.

I had posted previously on basic database connectivity with PowerShell [here][external-link] and using that code as a basis I wrote a PowerShell script that connects to the SQL database and extracts the data into an SQLDataAdapter. The required table is selected and imported into a hash table, and then exported to a CSV file. This file is then attached to an email using the .NET SMTP Mail object and sent on its way via SMTP.

{% highlight powershell linenos %}
#Connection Strings
$Database = "Database"
$Server = "SQLServer"
#SMTP Relay Server
$SMTPServer = "smtp.domain.com">
#Export File
$AttachmentPath = "C:\SQLData.csv"
# Connect to SQL and query data, extract data to SQL Adapter
$SqlQuery = "SELECT * FROM dbo.Test_Table"
$SqlConnection = New-Object System.Data.SqlClient.SqlConnection
$SqlConnection.ConnectionString = "Data Source=$Server;Initial Catalog=$Database;Integrated Security = True"
$SqlCmd = New-Object System.Data.SqlClient.SqlCommand
$SqlCmd.CommandText = $SqlQuery
$SqlCmd.Connection = $SqlConnection
$SqlAdapter = New-Object System.Data.SqlClient.SqlDataAdapter
$SqlAdapter.SelectCommand = $SqlCmd
$DataSet = New-Object System.Data.DataSet
$nRecs = $SqlAdapter.Fill($DataSet)
$nRecs | Out-Null
#Populate Hash Table
$objTable = $DataSet.Tables[0]
#Export Hash Table to CSV File
$objTable | Export-CSV $AttachmentPath
#Send SMTP Message
$Mailer = new-object Net.Mail.SMTPclient($SMTPServer)
$From = "email1@domain.com"
$To = "email2@domain.com"
$Subject = "Test Subject"
$Body = "Body Test"
$Msg = new-object Net.Mail.MailMessage($From,$To,$Subject,$Body)
$Msg.IsBodyHTML = $False
$Attachment = new-object Net.Mail.Attachment($AttachmentPath)
$Msg.attachments.add($Attachment)>$Mailer.send($Msg)
{% endhighlight %}

Hope this helps.

All information is provided on an AS-IS basis, with no warranties and confers no rights.

[external-link]: http://www.technogist.com/2009/04/connecting-to-a-sql-database/
