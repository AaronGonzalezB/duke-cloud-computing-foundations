# duke-cloud-computing-foundations
Repo of the Cloud Computing Foundations course

# Build Multiple Types of Websites

### 1. Static Website with S3
*An Amazon object storage.*

a. In Cloud 9 create an `index.html` file with the template that you want to show:

```
<html>
    <title>This is a website</title>
    <p>This is my new website!</p>
</html>
```

b. Create a new S3 bucket `hellocloud4data` for example:
  * Enable the public access
  * In the S3 menu, select your new bucket, in Properties Enable the `Static website hosting`
  * Assgin the `index.html` in the `Index document` and `error.htmtl` in the `Error document`
  * Go to `Permissions - Bucket policy` and set up the access to objects in the bucket:
  
  ```
  {
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::aaghellocloud4data/*"
        }
    ]
  }
  ```
c. Download the `index.html` and insert as an Object to the Bucket.

d. In `Properties - Static website hosting`, you can see your Bucket website endpoint, That's your URL hosting.

### 2. Website with AWS Lambda
*A serverless technology.*

a. In Cloud9, with the AWS Toolkit enabled, Create a Lambda function `Creatr Lambda SAM Application`.
  * Select a SAM Application Runtime - `python3.7`.
  * Select a SAM Application Template - `AWS SAM Hell World`.
  * Select a workspace for your new project.
  * Enter a name for your new application.

b. In the folder created, let's set up a raw html code in the `lambda handler`:

```
import json

# import requests


def lambda_handler(event, context):
    content = """
    <html>
        <p>Hello website Lambda</p>
    </hmtl>
    """
    response = {
        "statusCode": 200,
        "body": content,
        "headers": {"Content-Type" : "text/html",},
    }
    
    return response
```

c. Let's run it locally, for this we have to deploy our lambda function:
  * In `Preferences - AWS Settings` - Disable the `AWS Toolkit`, this allows you to run the lambda function in `AWS Resources`.
  * In `AWS Resources - Lambda Function` - go over the directory of the lambda function and in the `Run` option, clic in `Run API Gateway`
  * Click in the Run icon to see the Exectuion results, in this case the raw html code that we put into the `lambda_handler` function.

d. To deploy the website:
  * In `AWS Resources - Lambda Function` - go over the directory of the lambda function and click in `Deploy`.
  * Go to the Lambda functions menu, select the Function created and scroll down into the `API Gateway` Trigger.
  * The `API endpoint` it's your URL website.


3. EC2 - Virtual Machine
4. AWS Beanstalk - PaaS
