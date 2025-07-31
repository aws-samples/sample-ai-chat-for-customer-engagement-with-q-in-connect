# Q4C CloudFormation Templates

## Deploy Guide

There are 2 AWS Cloudformation tempaltes to deploy, starting with the `instance` template then the `main`. The ConnectInstanceId will be provided as an output of the `instance` template. This is used as an input parameter of the `main` template. 

1. `q4c-template-instance.yaml`
2. `q4c-template-main.yaml`

### Quick Deployment

- Quick deploy instance
  1. [Download template](https://raw.githubusercontent.com/aws-samples/sample-ai-chat-for-customer-engagement-with-q-in-connect/refs/heads/main/q4c-template-instance.yaml)
  2. [![Launch Stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=q-for-customers-instance)

- Quick deploy main
  1. [Download template](https://raw.githubusercontent.com/aws-samples/sample-ai-chat-for-customer-engagement-with-q-in-connect/refs/heads/main/q4c-template-main.yaml)
  2. [![Launch Stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=q-for-customers-main)

### Console Deployment

If you prefer to do it directly please review the AWS CloudFormation documentation.

- [Create a stack from the AWS CloudFormation console](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-create-stack.html)

### CLI Deployment

Deploy the built templates using AWS CLI:

```bash
aws cloudformation deploy --template-file q4c-template-instance.yaml --stack-name q4c-instance
```

```bash
INSTANCE_ID=$(aws cloudformation describe-stacks --stack-name q4c-instance --query 'Stacks[0].Outputs[?OutputKey==`ConnectInstanceId`].OutputValue' --output text)
echo ${INSTANCE_ID}
```

```bash
aws cloudformation deploy --template-file q4c-template-main.yaml --stack-name q4c-main --parameter-overrides ConnectInstanceId=${INSTANCE_ID}
```

## Parameters

| Name              | Type               | Description                        | Required |
| ----------------- | ------------------ | ---------------------------------- | -------- |
| ConnectInstanceId | String             | Existing Connect Instance Id       | **Yes**  |
| WebsiteUrl        | String             | URL for the website to scrape      | **Yes**  |
| DeploymentId      | String             | Unique ID for multiple deployments | No       |
| RemovalPolicy     | *`Delete`/`Retain` | CloudFormation Removal Policy      | No       |

*default