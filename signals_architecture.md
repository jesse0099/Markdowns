# Overview

![Infraestructure Diagram](https://drive.google.com/uc?id=1Of6KIhYgII2ElKOE1cca-Xbbba8Ds4-C)

## Resources

### Template

***[IAC](https://github.com/gitMLS/aws_signlas_infraestructure)***

The template can be used to create a similar infrastructure. The currently managed states will be uploaded to an S3 file. Instructions for creating a new stage, similar infrastructure, and/or accessing the currently deployed infrastructure will be provided in a further section on this page.


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


