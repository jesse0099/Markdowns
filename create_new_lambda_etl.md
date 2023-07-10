# Create New Lambda for ETL

The complete process of deploying ETL is documented on this [Confluence page](https://buyermls.atlassian.net/wiki/spaces/BS/pages/438992915/Deployment) (for general guidance). This section aims to provide guidance for developers who need to compose a new Lambda function, which involves crafting a `serverless.yml` file essential to the deployment procedure. Apart from this file, developers need not worry about any other files unrelated to programming logic.

This code snippet demonstrates the essential requirements when defining a new Lambda function ready to be deployed on different stages. Notice that any value surrounded by **{}** should be replaced with your desired ones **({place_holder} => ***actual_value***)**. Any other value should be kept. Additions to the functions section should follow the [official documentation](https://www.serverless.com/framework/docs/providers/aws/guide/functions), as well as for [events](https://www.serverless.com/framework/docs/providers/aws/guide/events), both sections that you probably will modify to fit your requirements.

## To replace values restriction and brief description: 

- **Lambda events:** 
The handler property points to the file and module containing the code you want to run in your function.

- **{service-name}:**
Placeholder for the name of the service.
It is recommended to use lowercase letters, numbers, and hyphens for the service name.

- **{image-tag-name}:**
Placeholder for the name of the Docker image tag.
It is recommended to use lowercase letters, numbers, and hyphens for the image tag name.
The image tag name value doesn't need to reference any existing resource; it will be used by serverless to manage the container registry images. It only needs to be compliant with the naming convention.

- **{aws_valid_region}:**
Placeholder for the AWS region.
Use the official AWS region names (e.g., us-east-1, eu-west-2).

- **{version}:**
Placeholder for the version of the application or service.
You can use alphanumeric characters or semantic versioning format (e.g., 1.0.0).

- **{function-name}:**
Placeholder for the name of the function.
It is recommended to use lowercase letters, numbers, and hyphens for the function name.

- **{app.command}:**
Placeholder for the command to be executed by the function's Docker container.
The actual value would depend on your application's requirements. For example, module.method (uploader.upload). The method is the one intended to be executed on lambda event detection.

```yml
service: {service-name}
# Current Serverless Framework Core Version (Different from plugin version)
frameworkVersion: '3'
provider:
  name: aws
  ecr:
    images:
      {image-tag-name}: 
        path: ./
  stage: ${opt:stage, 'stage'}
  region: {aws-region}
  versionFunctions: false
  # Avoid changing this, if a modification is required, the DevOps team will make it or help you to do it.
  iamRoleStatements:
    - Effect: "Allow"
      Action:
        - cloudwatch:*
        - dynamodb:*
        - secretsmanager:*
        - ecs:*
        - iam:PassRole
        - events:*
        - logs:*
        - s3:*
        - es:*
        - ses:*
      Resource: "*"
  environment:

custom:
  stages:
    - stage
    - prod
  version: {version}

functions:
  {function-name}:
    image:
      name: {image-tag-name}
      command:
        - {app.command}
    name: ${self:custom.version}-${self:provider.stage}-{function-name}
    memorySize: 512
    timeout: 300
    vpc: ${file(./vpc.yml)}
    environment:
    events:

plugins:
  - serverless-stage-manager

package:
  exclude:
    - node_modules/**
    - etl/**
```