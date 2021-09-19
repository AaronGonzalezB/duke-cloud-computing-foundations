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


### 3. EC2 - Virtual Machine:

In a Cloud9 environment, we will create a .pem file that allows to connect via SSH with the VM and with the EC2 Spot Console, install a web service in the browser.

a. Setup a PEM key.
  * In the `EC2 - Key pairs - Create key pair` - assign a name an a `pem` File format.
  * A .pem file is downloaded, upload this file in the Clou9 environment, out of the folders already created.

b. Assign port labels:
  * Go to the `EC2 - Network and Security - Create a security group` - assign a name, in this case `hellospotwebservice`.
  * Add rule - setup an Inbound rule with Port range 22 and Source `0.0.0.0/0`
  * Add another rule with Port range 80 and Source `0.0.0.0/0`

c. Launch a new Instance:
  * Got to `EC2 - Spot Requests - Request Spot Instance - Big data workloads (optional) - AMI: Amazon Linux 2` - assign a `Key pair name` based on the Key pair created.
  * Assign the `security group` created
  * Launch the instance.
  * When the instance is running, assign a name, in this case `spot-web-service` then Connect the instance.

d. Connect to instance:
  * Inside the instance - SSH client - Launch the chmod command in the Cloud9 console: `chmod 400 hellowebservice.pem`
  * In Cloud9 console, connect with `ssh -i "hellowebservice.pem" ec2-user@ec2-3-89-86-148.compute-1.amazonaws.com`

e. Setup the VM:
  * Install and start the http server with:
    ```
    sudo yum update -y
    sudo yum install -y httpd
    sudo systemctl start httpd
    ```
  
f. Test the web service: 
  * Inside the Instance - EC2 Instace Connect - Instance ID - Open public IPv4 address.
  * Allow the EC2 user to become part of the Apache group - `sudo usermod -a -G apache ec2-user`
  * Assign a new permission level - `sudo chown -R ec2-user:apache /var/www`
  * add group write permissions: 
  ```
  sudo chmod 2775 /var/www && find /var/www -type d -exec sudo chmod 2775 {} \;
  find /var/www -type f -exec sudo chmod 0664 {} \;
  ```
  * go to the web service and add content on it:
  ```
  cd /var/www
  cd html
  touch index.html
  vim index.html
        <html>
            <p>Test the web service</p>
        </html>
  ```
  * Restart the web service and refresh the browser: `sudo systemctl restart httpd`
 
 That's it!

### 5. AWS Beanstalk - PaaS
To setup an environment we have to install the Ealist Beanstalk Command Line Interface (EB CLI) via the latest github repo from Amazon: https://github.com/aws/aws-elastic-beanstalk-cli-setup

Once we install, the Load Balancer give us the entire ecosystem to deploy our Flask application.





