---
layout: post
title: Terraform External Provider - Python
date: 2017-12-17 21:16:00 -07:00
categories: Terraform Python
---

Using an 'External Provider' in Terraform is pretty simple but there are specific requirements for the scripts processing the JSON query data. The documentation on hashicorp.com does show a bash shell example but nothing for Python. Below is an example for Python using the same premise but leveraging 'json' Python module to robustly transform the JSON data.

[Terraform External Data Provider][external-link]

The terraform external data provider passes JSON query data into the executed script via STDIN and expects a JSON formatted response back from STDOUT.

Example JSON input:

```
{
    "data": "Test123"
}
```

Example Provider:

{% highlight bash %}
data "external" "external_resource" {
  program = ["python", "${path.module}/python_script.py"]

  query = {
    data = "Test123"
  }
}
{% endhighlight %}

Example Script:

{% highlight python %}
#!/usr/bin/env python

"""Script to test the external data provider in Terraform"""

import sys
import json

def read_in():
    return {x.strip() for x in sys.stdin}

def main():
    lines = read_in()
    for line in lines:
        if line:
            jsondata = json.loads(line)
            jsondata["new-data"] = "Test123"
            sys.stdout.write(json.dumps(jsondata))

if __name__ == '__main__':
    main()
{% endhighlight %}

Go [here][external-link2] to pull down the module.

In the directory for the module run the below to prepare the module for execution; 'init', 'plan' and then 'apply'.

```
Tue Dec 19 16:05:18$ terraform init
...
Tue Dec 19 16:05:18$ terraform plan
...
Tue Dec 19 16:05:18$ terraform apply
...
```

The end result should be your altered JSON data returned as an output resource.

```
Tue Dec 19 16:05:18$ terraform apply

data.external.external_resource: Refreshing state...

Apply complete! Resources: 0 added, 0 changed, 0 destroyed.

Outputs:

external_result = {
  data = Test123
  new-data = Test123
}
```

The above example stores the input from STDIN and then uses the Python 'json' module to translate from string to JSON in a robust way. The JSON object is apended with the new data and sent back to Terraform by 'stringyfying' it and writing to STDOUT. This can be used to perform any external processing as part of a Terraform module.

Hope it helps.

All information is provided on an AS-IS basis, with no warranties and confers no rights

[external-link]: https://www.terraform.io/docs/providers/external/data_source.html
[external-link2]: https://github.com/paulbrice/aws/tree/master/terraform/external_provider
