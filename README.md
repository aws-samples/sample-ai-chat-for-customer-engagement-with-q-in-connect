# Q4C CloudFormation Templates

## Quick Start

For rapid deployment:

1. **Deploy Instance Template First**
   -  <a href="https://raw.githubusercontent.com/aws-samples/sample-ai-chat-for-customer-engagement-with-q-in-connect/refs/heads/main/q4c-template-instance.yaml" download>Download instance template</a>
   - [![Launch Stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=q-for-customers-instance)

2. **Deploy Main Template Second**
   - <a href="https://raw.githubusercontent.com/aws-samples/sample-ai-chat-for-customer-engagement-with-q-in-connect/refs/heads/main/q4c-template-main.yaml" download>Download main template</a>
   - [![Launch Stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home#/stacks/new?stackName=q-for-customers-main)

Use the ConnectInstanceId output from the instance template as input for the main template.

## Overview

There are 2 AWS CloudFormation templates to deploy, starting with the `instance` template then the `main`. The ConnectInstanceId will be provided as an output of the `instance` template. This is used as an input parameter of the `main` template. 

1. `q4c-template-instance.yaml`
2. `q4c-template-main.yaml`

## Parameters

| Name | Type | Description |
| ----- | ----- | ----- |
| **General** |
| DeploymentId | String | Unique identifier for this deployment | 
| ConnectInstanceId | String | The ID of the Connect instance (E.g. 00000000-aaaa-bbbb-cccc-111111111111) |
| RemovalPolicy | *`Delete`/`Retain` | Removal policy for resources |
| **Web Scraper** |
| WebsiteUrl | String | Website URL to scrape for knowledge base (E.g. https://example.com) |
| ExclusionFilter | CommaDelimitedList | Array of regex patterns to exclude URLs from scraping |
| InclusionFilter | CommaDelimitedList | Array of regex pattern to include URLs from scraping. (Leave blank for anything not excluded) |
| ScopeFilter | String | Scope for URL crawling |

*default

## Deployment Options

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
