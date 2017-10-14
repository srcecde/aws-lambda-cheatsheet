# AWS Lambda Cheatsheet

<h3>This cheatsheet is probably based on Python</h3>
<br>
<table>
  <tr>
    <th colspan="2">Runtime Versions</th>
  </tr>
  <tr>
    <th>Type</th><th>Versions</th>
  </tr>
  <tr>
    <td>Node.js</td><td>v6.4.2 and v6.10.3</td>
  </tr>
  <tr>
    <td>Java</td><td>Java 8</td>
  </tr>
  <tr>
    <td>Python</td><td>v2.7 and v3.6</td>
  </tr>
  <tr>
    <td>.NET Core</td><td>.NET Core 1.0.1 (C#)</td>
  </tr>
</table>
<br>
<table>
  <tr>
    <th>Available library for Python Execution Envionment</th>
  </tr>
  <tr>
    <td>AWS SDK for Python 2.7 (Boto 3) version 3-1.4.7 botocore-1.7.2</td>
  </tr>
  <tr>
    <td>AWS SDK for Python 3.6 (Boto 3) version 3-1.4.7 botocore-1.7.2</td>
  </tr>
</table>
<br>
<table>
  <tr>
    <th colspan="2">Settings</th>
  </tr>
  <tr>
    <td><b>Writable Path</b></td>
    <td>/tmp/</td>
  </tr>
  <tr>
    <td><b>Default</b></td>
    <td>128 MB Memory 3 Second Timeout</td>
  </tr>
  <tr>
    <td><b>Modified (Max Limit)</b></td>
    <td>1536 MB Memory 5 Minutes Timeout</td>
  </tr>
</table>
<br>
<table>
  <tr>
    <th colspan=2>Execution Role (Common Execution Role Available)</th>
  </tr>
  <tr>
    <td><b>AWSLambdaBasicExecutionRole</b></td>
    <td>Grants permissions only for the Amazon CloudWatch Logs actions to write logs.</td>
  </tr>
  <tr>
    <td><b>AWSLambdaKinesisExecutionRole</b></td>
    <td>Grants permissions for Amazon Kinesis Streams actions, and CloudWatch Logs actions.</td>
  </tr>
  <tr>
    <td><b>AWSLambdaDynamoDBExecutionRole</b></td>
    <td>Grants permissions for DynamoDB streams actions and CloudWatch Logs actions.</td>
  </tr>
  <tr>
    <td><b>AWSLambdaVPCAccessExecutionRole</b></td>
    <td>Grants permissions for Amazon Elastic Compute Cloud (Amazon EC2) actions to manage elastic network interfaces (ENIs).</td>
  </tr>
</table>
<br>
<b>Add new permission</b>

    import boto3
    client = boto3.client('lambda')
    
    # Role ARN can be found on the top right corner of the Lambda function
    response = client.add_permission(
        FunctionName='string',
        StatementId='string',
        Action='string',
        Principal='string',
        SourceArn='string',
        SourceAccount='string',
        EventSourceToken='string',
        Qualifier='string'
    )
<br>
<table>
  <tr>
    <th colspan="2">Execution | Invoke | Tweaks</th>
  </tr>
  <tr>
    <td>A Lambda can invoke another Lambda</td>
    <td>Yes</td>
  </tr>
  <tr>
    <td>A Lambda in one region can invoke another lambda in other region</td>
    <td>Yes</td>
  </tr>
  <tr>
    <td>A Lambda can invoke same Lambda</td>
    <td>Yes</td>
  </tr>
  <tr>
    <td>Exceed 5 minutes execution time</td>
    <td>Yes (Can Tweak around)</td>
  </tr>
  <tr>
    <td>How to exceed 5 minutes execution time</td>
    <td> <a href="http://www.thetechnologyupdates.com/aws-lambda-going-beyond-5-minutes-execution/">Self-Invoke</a> , SNS, SQS</td>
  </tr>
  <tr>
    <td>Asynchronous Execution</td>
    <td>Yes <a href="http://www.thetechnologyupdates.com/aws-lambda-execute-lambda-asynchronously-parallel/">(Async Exec)</a></td>
  </tr>
  <tr>
    <td>Invoke same Lamba with different version</td>
    <td>Yes</td>
  </tr>
</table>
<br>
<b>Setting Lambda Invoke Max Retry attempt to 0</b>

    import boto3, botocore
    config = botocore.config.Config(connect_timeout=300, read_timeout=300)
    invokeLam = boto3.client('lambda', region_name='us-east-1', config=config)
    invokeLam.meta.events._unique_id_handlers['retry-config-lambda']['handler']._checker.__dict__['_max_attempts'] = 0
<br>
<table>
  <tr>
    <th>Triggers</th>
    <th>Description</th>
    <th>Requirement</th>
  </tr>
  <tr>
    <td><b>API Gateway</b></td>
    <td>Trigger AWS Lambda function over HTTPS</td>
    <td>API Endpoint name<br>API Endpoint Deployment Stage<br>Security Role</td>
  </tr>
  <tr>
    <td><b>AWS IoT</b></td>
    <td>Trigger AWS Lambda for performing specific action by mapping your AWS IoT Dash Button (Cloud Programmable Dash Button)</td>
    <td>DSN (Device Serial Number)</td>
  </tr>
  <tr>
    <td><b>Alexa Skill Kit</b></td>
    <td>Trigger AWS Lambda to build services that give new skills to Alexa</td>
    <td>--</td>
  </tr>
  <tr>
    <td><b>Alexa Smart Home</b></td>
    <td>Trigger AWS Lambda with desired skill</td>
    <td>Application ID (Skill)</td>
  </tr>
  <tr>
    <td><b>CloudWatch Events</b></td>
    <td>Trigger AWS Lambda on desired time interval (rate(1 day)) or on the state change of EC2, RDS, S3, Health.</td>
    <td>Rule based on either Event Pattern (time interval)<br>Schedule Expression (Auto Scaling on events like Instance launch and terminate<br>AWS API call via CloudTrail</td>
  </tr>
  <tr>
    <td><b>CloudWatch Logs</b></td>
    <td>Trigger AWS Lambda based on the CloudWatch Logs</td>
    <td>Log Group Name</td>
  </tr>
  <tr>
    <td><b>Code Commit</b></td>
    <td>Trigger AWS Lambda based on the AWS CodeCommit version control system</td>
    <td>Repository Name<br>Event Type</td>
  </tr>
  <tr>
    <td><b>Cognito Sync Trigger</b></td>
    <td>Trigger AWS Lambda in response to event, each time the dataset is synchronized</td>
    <td>Cognito Identity Pool dataset</td>
  </tr>
  <tr>
    <td><b>DynamoDB</b></td>
    <td>Trigger AWS Lambda whenever the DynomoDB table is updated</td>
    <td>DynamoDB Table name<br>Batch Size(The largest number of records that AWS Lambda will retrieve from your table at the time of invoking your function. Your function receives an event with all the retrieved records)</td>
  </tr>
  <tr>
    <td><b>Kinesis</b></td>
    <td>Trigger AWS Lambda whenever the Kinesis stream is updated</td>
    <td>Kinesis Stream<br>Batch Size</td>
  </tr>
  <tr>
    <td><b>S3</b></td>
    <td>Trigger AWS Lambda in response to file dropped in S3 bucket</td>
    <td>Bucket Name<br>Event Type (Object Removed, Object Created)</td>
  </tr>
  <tr>
    <td><b>SNS</b></td>
    <td>Trigger AWS Lambda whenever the message is published to Amazon SNS Topic</td>
    <td>SNS Topic</td>
  </tr>
</table>
<br>
<h4>References</h4>
<ul>
  <li><a href="https://aws.amazon.com/documentation/lambda/">AWS Lambda Docs</a></li>
  <li><a href="https://boto3.readthedocs.io/en/latest/">Boto 3 Docs</a></li>
</ul>
<br>
<b>For queries or issues, feel free to contact or open an <a href="https://github.com/srcecde/aws-lambda-cheatsheet/issues">issue</a></b>
