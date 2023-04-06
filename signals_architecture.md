# Overview

![alt text](https://drive.google.com/uc?id=1Of6KIhYgII2ElKOE1cca-Xbbba8Ds4-C)

## Resources

All resources managed by this template are tagged and named following naming conventions described in the next confluence page [AWS Tagging/Naming Conventions](https://buyermls.atlassian.net/wiki/spaces/PER/pages/2706374657/AWS+Tagging+Naming+Conventions) with the following values for cost tracking and ease of querying related ones:

- **Project :** signals
- **CostCode :** signals
- **Environment :** dev, prod
- **Created_By :** Extracted from the last deploy associated IAM user.
- **Terraform :** https://github.com/gitMLS/aws_signlas_infraestructure

### Template

***[IAC](https://github.com/gitMLS/aws_signlas_infraestructure)***

The template can be used to create a similar infrastructure. The currently managed states will be uploaded to an S3 bucket. Instructions for creating a new stage on the existing infrastructure, deploying similar infrastructure on another account, and/or accessing the currently deployed infrastructure will be provided in a further section on this page.

### List of AWS resources managed by this template:

* aws_api_gateway_api_key.signals_api_key
* aws_api_gateway_rest_api.signal_api
* aws_api_gateway_stage.signals_stage
* aws_api_gateway_usage_plan.signals_usage_plan
* aws_api_gateway_usage_plan_key.signals_usage_plan_key
* aws_cloudwatch_log_group.signals_log_group
* aws_iam_policy.firehose_s3_policy
* aws_iam_policy.kinesis_put_record_policy
* aws_iam_role.firehose_role
* aws_iam_role.kinesis_putrecord_role
* aws_iam_role.signal_redshift_role
* aws_kinesis_firehose_delivery_stream.signal_firehose
* aws_kinesis_stream.signal_stream
* aws_redshift_cluster.signals_redshift_cluster
* aws_s3_bucket.signal_bucket
* aws_security_group.signal_firehose_ingress

## Template usage

### Deploying new infrastructure


1. Download the repository content and install Terraform.

2. Create a terraforms.tfvars file

    Based on the terraforms.tfvars.example file, create the terraform.tfvars.
    For example:
    ```bash
    sudo cp terraforms.tfvars.example terraform.tfvars
    ```

3. Replace the values of the terraform.tfvars file.

    The variables.tf file contains the type and description of each variable, read the file
    and replace the values of terraform.tfvars as needed.

4. Deploy it.
    
    Deploy the infrastructure using the following commands:
    ```bash
    terraform init
    terraform plan 
    terraform apply
    ```

### Deploying new stage on existing infrastructure

1. Download the repository content and install Terraform.

2. Donwload the used terraform.tfvars file and the associated Terraform state management files

    Currently, no defined default location for this purpose exists. This section will be updated when the location is available.

3. Modify the terraforms.tfvars file.

    ```bash
    environment = "dev"
    ```
4. Create new Terraform workspace.

    ```bash
    terraform workspace new {workspace_name}
    ```

4. Deploy it.
    
    Deploy the infrastructure using the following commands:
    ```bash
    terraform init
    terraform plan 
    terraform apply
    ```


### Destroying stage on existing infrastructure

1. Download the repository content and install Terraform.

2. Donwload the used terraform.tfvars file and the associated Terraform state management files

    Currently, no defined default location for this purpose exists. This section will be updated when the location is available.

3. Select the Terraform workspace associated with the stage.

    ```bash
     terraform workspace select {workspace_name}
    ```

4. Destroy it.
    
    Destroy the stage using the following commands:
    ```bash
    terraform destroy
    ```