# Airtbale as the presentation layer.

As part of the efforts to maintain clear, easy-to-access, and easy-to-query documentation for infrastructure resources, a set of scripts are being developed. 

Documentary scripts keep track of resources using **CSV** files uploaded to **S3 buckets**. Each script is executed at the beginning and end of the day, so all changes made over the day are available for inspection when needed.  

CSV is a convenient format for technical users, but it could not the most suitable for those who are not. Especially for information exposure.

**Airtable** acts as a presentation layer, where the last script execution reflects the state of resources, with the possibility to query them easily and format them in beautiful ways.

## Know more about the scripts and how to use them

Top-level and detailed documentation about integrating **Airtable** with the scripts is available in each tool's **README** file.

***To see repository content, granted access is required beforehand.***

- [Installation And Usage](https://ec2-instance-descriptor.readthedocs.io/en/latest/examples.html#installation-usage)

- [Integration](https://github.com/jesse0099/EC2_INSTANCE_DESCRIPTOR#readme)

- [Repository ](https://github.com/gitMLS/percy-liberty-devops-tools)

