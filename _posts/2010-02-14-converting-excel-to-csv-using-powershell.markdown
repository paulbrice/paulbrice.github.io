---
layout: post
title: Converting Excel to CSV using PowerShell
date: 2010-02-14 21:56:00 -07:00
categories: csv excel
---
Quick Script: Converting Excel[.xls] to Comma Separated Value[.csv]
Many times my data is in .xls format and I really need it in .csv to leverage the accessibility of the 'Import-CSV' cmdlet.
I found this code on the 'PowerShellCommunity.org' web forum: [here][external-link]

{% highlight powershell linenos%}
$xlCSV=6
$Excelfilename = "file.xls"
$CSVfilename = "file.csv"
$Excel = New-Object -comobject Excel.Application$Excel.Visible = $False
$Excel.displayalerts=$False
$Workbook = $Excel.Workbooks.Open($ExcelFileName)
$Workbook.SaveAs($CSVfilename,$xlCSV)
$Excel.Quit()
If(ps excel){kill -name excel}
{% endhighlight %}

I have only tested this with Office 2003 Sp2. I will try with 2007/2010 and post my findings.

Hope this helps.

All information is provided on an AS-IS basis, with no warranties and confers no rights.

[external-link]: http://powershellcommunity.org/Forums/tabid/54/aff/1/aft/657/afv/topic/Default.aspx
