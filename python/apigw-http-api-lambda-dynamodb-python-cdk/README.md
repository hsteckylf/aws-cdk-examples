
# AWS API Gateway HTTP API to AWS Lambda in VPC to DynamoDB CDK Python Sample!


## Overview

Creates an [AWS Lambda](https://aws.amazon.com/lambda/) function writing to [Amazon DynamoDB](https://aws.amazon.com/dynamodb/) and invoked by [Amazon API Gateway](https://aws.amazon.com/api-gateway/) REST API. 

![architecture](docs/architecture.png)

## Throttling Configuration

This API implements AWS Well-Architected Framework best practice **REL05-BP02: Throttle requests** with the following limits:

**Stage-Level Throttling:**
- Rate limit: 100 requests/second
- Burst limit: 200 requests

**Per-Client Throttling (Usage Plan):**
- Rate limit: 50 requests/second
- Burst limit: 100 requests

**Benefits:**
- Protects against unexpected traffic spikes and retry storms
- Prevents resource exhaustion of Lambda and DynamoDB
- Enables per-client rate limiting via API keys
- Returns HTTP 429 (Too Many Requests) when limits exceeded

**API Key Usage:**
After deployment, retrieve your API key value:
```bash
aws apigateway get-api-key --api-key <API_KEY_ID> --include-value
```

Include the API key in requests:
```bash
curl -X POST https://<api-id>.execute-api.<region>.amazonaws.com/prod/ \
  -H "x-api-key: <your-api-key>" \
  -H "Content-Type: application/json" \
  -d '{"year":"2023","title":"Example","id":"123"}'
```

## Setup

The `cdk.json` file tells the CDK Toolkit how to execute your app.

This project is set up like a standard Python project.  The initialization
process also creates a virtualenv within this project, stored under the `.venv`
directory.  To create the virtualenv it assumes that there is a `python3`
(or `python` for Windows) executable in your path with access to the `venv`
package. If for any reason the automatic creation of the virtualenv fails,
you can create the virtualenv manually.

To manually create a virtualenv on MacOS and Linux:

```
$ python3 -m venv .venv
```

After the init process completes and the virtualenv is created, you can use the following
step to activate your virtualenv.

```
$ source .venv/bin/activate
```

If you are a Windows platform, you would activate the virtualenv like this:

```
% .venv\Scripts\activate.bat
```

Once the virtualenv is activated, you can install the required dependencies.

```
$ pip install -r requirements.txt
```

At this point you can now synthesize the CloudFormation template for this code.

```
$ cdk synth
```

To add additional dependencies, for example other CDK libraries, just add
them to your `setup.py` file and rerun the `pip install -r requirements.txt`
command.

## Deploy
At this point you can deploy the stack. 

Using the default profile

```
$ cdk deploy
```

With specific profile

```
$ cdk deploy --profile test
```

## After Deploy
Navigate to AWS API Gateway console and test the API with below sample data 
```json
{
    "year":"2023", 
    "title":"kkkg",
    "id":"12"
}
```

You should get below response 

```json
{"message": "Successfully inserted data!"}
```

## Cleanup 
Run below script to delete AWS resources created by this sample stack.
```
cdk destroy
```

## Useful commands

 * `cdk ls`          list all stacks in the app
 * `cdk synth`       emits the synthesized CloudFormation template
 * `cdk deploy`      deploy this stack to your default AWS account/region
 * `cdk diff`        compare deployed stack with current state
 * `cdk docs`        open CDK documentation

Enjoy!
