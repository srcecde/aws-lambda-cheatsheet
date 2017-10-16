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
    <th colspan="2">Settings | Limits</th>
  </tr>
  <tr>
    <td><b>Writable Path & Space</b></td>
    <td>/tmp/ 512 MB</td>
  </tr>
  <tr>
    <td><b>Default Memory & Execution Time</b></td>
    <td>128 MB Memory<br>3 Second Timeout</td>
  </tr>
  <tr>
    <td><b>Max Memory & Execution Time</b></td>
    <td>1536 MB Memory<br>5 Minutes Timeout</td>
  </tr>
  <tr>
    <td><b>Number of processes and threads (Total)</b></td>
    <td>1024</td>
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
  <tr>
    <td>Invoke Request Max Payload Size (RequestResponse/synrchronous invocation)</td>
    <td>6 MB</td>
  </tr>
  <tr>
    <td>Invoke Request Max Payload Size (Event/asynchronous invocation)</td>
    <td>128 KB</td>
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
<table>
  <tr>
    <th colspan="3">Troubleshooting</th>
  </tr>
  <tr>
    <th>Error</th>
    <th>Possible Reason</th>
    <th>Solution</th>
  </tr>
  <tr>
    <td>
    File "/var/task/lambda_function.py", line 2, in lambda_handler<br>return event['demoevent']<br>
    KeyError: 'demoevent'
    </td>
    <td>
      Event does not have the key 'demoevent' or either misspelled
    </td>
    <td>
      Make sure the event is getting the desired key if it is receiving the event from any trigger.<br> Or if the not outside event is passed than check for misspell.<br>Or check the event list by printing event.
    </td>
  </tr>
  <tr>
    <td>botocore.exceptions.ClientError: An error occurred (AccessDeniedException) when calling the GetParameters operation: User: arn:aws:dummy::1234:assumed-role/role/ is not authorized to perform: ssm:GetParameters on resource: arn:aws:ssm:dummy</td>
    <td>Lacks Permission to access</td>
    <td>Assign appropriate permission for accessibility</td>
  </tr>
  <tr>
    <td>ImportError: Missing required dependencies [‘module']</td>
    <td>Dependent module is missing</td>
    <td>Install/Upload the required module</td>
  </tr>
  <tr>
    <td>sqlalchemy.exc.OperationalError: (psycopg2.OperationalError) could not translate host name "host.dummy.region.rds.amazonaws.com" to address: Name or service not known</td>
    <td>RDS Host is unavailable</td>
    <td>Make sure the RDS instance is up and running.<br>Double check the RDS hostname</td>
  </tr>
  <tr>
    <td>[Errno 32] Broken pipe</td>
    <td>Connection is lost (Either from your side or may be some problem from AWS)<br>While invoking another Lambda, if the payload size exceed the mentioned limit</td>
    <td>Make sure if you are passing the payload of right size.<br>Check for the connection.</td>
  </tr>
  <tr>
    <td>Unable to import module ‘lambda_function/index’ No module named ‘lambda_function'</td>
    <td>Handler configuration is not matching the main file name</td>
    <td>Update the handler configuration as per your filename.function_name</td>
  </tr>
</table>
<br>
<h3>AWS Lambda CLI commands</h3>
<br>
<b>Add Permission</b>
<p>It add mention permission to the Lambda function</p>
<p>Syntax</p>

      add-permission
    --function-name <value>
    --statement-id <value>
    --action <value>
    --principal <value>
    [--source-arn <value>]
    [--source-account <value>]
    [--event-source-token <value>]
    [--qualifier <value>]
    [--cli-input-json <value>]
    [--generate-cli-skeleton <value>]

<p>Example</p>

    add-permission --function-name functionName --statement-id role-statement-id --action lambda:CreateFunction --principal s3.amazonaws.com
<br>
<b>Create Alias</b>
<p>It creates alias for the given Lambda function name</p>
<p>Syntax</p>

      create-alias
    --function-name <value>
    --name <value>
    --function-version <value>
    [--description <value>]
    [--cli-input-json <value>]
    [--generate-cli-skeleton <value>]

<p>Example</p>

    create-alias --function-name functionName --name fliasName --function-version version
<br>
<b>Create Event Source Mapping</b>
<p>It identify event-source from Amazon Kinesis stream or an Amazon DynamoDB stream</p>

      create-event-source-mapping
    --event-source-arn <value>
    --function-name <value>
    [--enabled | --no-enabled]
    [--batch-size <value>]
    --starting-position <value>
    [--starting-position-timestamp <value>]
    [--cli-input-json <value>]
    [--generate-cli-skeleton <value>]

<p>Example</p>

    create-event-source-mapping --event-source-arn arn:aws:kinesis:us-west-1:1111 --function-name functionName --starting-position LATEST

<br>
<b>Create Function</b>
<p>It creates the new function</p>
<p>Syntax</p>

      create-function
    --function-name <value>
    --runtime <value>
    --role <value>
    --handler <value>
    [--code <value>]
    [--description <value>]
    [--timeout <value>]
    [--memory-size <value>]
    [--publish | --no-publish]
    [--vpc-config <value>]
    [--dead-letter-config <value>]
    [--environment <value>]
    [--kms-key-arn <value>]
    [--tracing-config <value>]
    [--tags <value>]
    [--zip-file <value>]
    [--cli-input-json <value>]
    [--generate-cli-skeleton <value>]
<p>Example</p>

    create-function --function-name functionName --runtime python3.6 --role arn:aws:iam::account-id:role/lambda_basic_execution
     --handler main.handler
<br>
<b>Delete Alias</b>
<p>It deletes the alias</p>
<p>Syntax</p>

      delete-alias
    --function-name <value>
    --name <value>
    [--cli-input-json <value>]
    [--generate-cli-skeleton <value>]
<p>Example</p>

    delete-alias --function-name functionName --name aliasName
<br>
<b>Delete Event Source Mapping</b>
<p>It deletes the event source mapping</p>
<p>Syntax</p>

      delete-event-source-mapping
    --uuid <value>
    [--cli-input-json <value>]
    [--generate-cli-skeleton <value>]
<p>Example</p>
    
    delete-event-source-mapping --uuid 12345kxodurf3443
<br>
<b>Delete Function</b>
<p>It will delete the function and all the associated settings</p>
<p>Syntax</p>

      delete-function
    --function-name <value>
    [--qualifier <value>]
    [--cli-input-json <value>]
    [--generate-cli-skeleton <value>]
<p>Example</p>

    delete-function --function-name FunctionName
<br>
<b>Get Account Settings</b>
<p>It will fetch the user’s account settings</p>
<p>Syntax</p>

      get-account-settings
    [--cli-input-json <value>]
    [--generate-cli-skeleton <value>]

<br>
<b>Get Alias</b>
<p>It returns the desired alias information like description, ARN</p>
<p>Syntax</p>

      get-alias
    --function-name <value>
    --name <value>
    [--cli-input-json <value>]
    [--generate-cli-skeleton <value>]
<p>Example</p>

    get-alias --function-name functionName --name aliasName
<br>
<b>Get Event Source Mapping</b>
<p>It returns the config information for the desired event source mapping</p>
<p>Syntax</p>

      get-event-source-mapping
    --uuid <value>
    [--cli-input-json <value>]
    [--generate-cli-skeleton <value>]
<p>Example</p>
    
    get-event-source-mapping --uuid 12345kxodurf3443
<br>
<b>Get Function</b>
<p>It returns the Lambda Function information</p>
<p>Syntax</p>

      get-function
    --function-name <value>
    [--qualifier <value>]
    [--cli-input-json <value>]
    [--generate-cli-skeleton <value>]
<p>Example</p>
    
    get-function --function-name functionName
<br>
<b>Get Function Configuration</b>
<p>It returns the Lambda function configuration</p>
<p>Syntax</p>

      get-function-configuration
    --function-name <value>
    [--qualifier <value>]
    [--cli-input-json <value>]
    [--generate-cli-skeleton <value>]
<p>Example</p>

      get-function-configuration --function-name functionName
<br>
<b>Get Policy</b>
<p>It return the linked policy with Lambda function</p>
<p>Syntax</p>

      get-policy
    --function-name <value>
    [--qualifier <value>]
    [--cli-input-json <value>]
    [--generate-cli-skeleton <value>]
<p>Example</p>

    get-policy --function-name functionName
<br>
<b>Invoke</b>
<p>It invoke the mention Lambda function name</p>
<p><Syntax/p>

      invoke
    --function-name <value>
    [--invocation-type <value>]
    [--log-type <value>]
    [--client-context <value>]
    [--payload <value>]
    [--qualifier <value>]
<p>Example</p>

    invoke --function-name functionName
<br>
<b>List Aliases</b>
<p>It return all the aliases that is created for Lambda function</p>
<p>Syntax</p>

      list-aliases
    --function-name <value>
    [--function-version <value>]
    [--marker <value>]
    [--max-items <value>]
    [--cli-input-json <value>]
    [--generate-cli-skeleton <value>]
<p>Example</p>

      list-aliases --function-name functionName
<br>
<b>List Event Source Mappings</b>
<p>It return all the list event source mappings that is created with create-event-source-mapping</p>
<p>Syntax</p>

      list-event-source-mappings
    [--event-source-arn <value>]
    [--function-name <value>]
    [--max-items <value>]
    [--cli-input-json <value>]
    [--starting-token <value>]
    [--page-size <value>]
    [--generate-cli-skeleton <value>]
<p>Example</p>

      list-event-source-mappings --event-source-arn arn:aws:arn --function-name functionName
<br>
<b>List Functions</b>
<p>It return all the Lambda function</p>
<p>Syntax</p>

      list-functions
    [--master-region <value>]
    [--function-version <value>]
    [--max-items <value>]
    [--cli-input-json <value>]
    [--starting-token <value>]
    [--page-size <value>]
    [--generate-cli-skeleton <value>]
<p>Example</p>

      list-functions --master-region us-west-1 --function-version ALL
<br>
<b>List Tags</b>
<p>It return the list of tags that are assigned to the Lambda function</p>
<p>Syntax</p>

      list-tags
    --resource <value>
    [--cli-input-json <value>]
    [--generate-cli-skeleton <value>]
<p>Example</p>

      list-tags --resource arn:aws:function
<br>
<b>List Versions by functions</b>
<p>It return all the versions of the desired Lambda function</p>
<p>Syntax</p>

      list-versions-by-function
    --function-name <value>
    [--marker <value>]
    [--max-items <value>]
    [--cli-input-json <value>]
    [--generate-cli-skeleton <value>]
<p>Example</p>

    list-versions-by-function --function-name functionName
<br>
<b>Publish Version</b>
<p>It publish the version of the Lambda function from $LATEST snapshot</p>
<p>Syntax</p>

      publish-version
    --function-name <value>
    [--code-sha-256 <value>]
    [--description <value>]
    [--cli-input-json <value>]
    [--generate-cli-skeleton <value>]
<p>Example</p>

      publish-version --function-name functionName
<br>
<b>Remove Permission</b>
<p>It remove the single permission from the policy that is linked with the Lambda function</p>
<p>Syntax</p>

     remove-permission
    --function-name <value>
    --statement-id <value>
    [--qualifier <value>]
    [--cli-input-json <value>]
    [--generate-cli-skeleton <value>]
<p>Example</p>

     remove-permission --function-name functionName --statement-id role-statement-id
<br>
<b>Tag Resource</b>
<p>It creates the tags for the lambda function in the form of key-value pair</p>
<p>Syntax</p>

      tag-resource
    --resource <value>
    --tags <value>
    [--cli-input-json <value>]
    [--generate-cli-skeleton <value>]
<p>Example</p>

    tag-resource --resource arn:aws:arn --tags {‘key’: ‘pair’}
<br>
<b>Untag Resource</b>
<p>It remove tags from the Lambda function</p>
<p>Syntax</p>

     untag-resource
    --resource <value>
    --tag-keys <value>
    [--cli-input-json <value>]
    [--generate-cli-skeleton <value>]
<p>Example</p>

    untag-resource --resource arn:aws:complete --tag-keys [‘key1’, ‘key2’]
<br>
<b>Update Alias</b>
<p>It update the alias name of the desired lambda function</p>
<p>Syntax</p>

      update-alias
    --function-name <value>
    --name <value>
    [--function-version <value>]
    [--description <value>]
    [--cli-input-json <value>]
    [--generate-cli-skeleton <value>]
<p>Example</p>

    update-alias --function-name functionName --name aliasName
<br>
<b>Update Event Source Mapping</b>
<p>It updates the event source mapping incase you want to change the existing parameters</p>
<p>Syntax</p>

      update-event-source-mapping
    --uuid <value>
    [--function-name <value>]
    [--enabled | --no-enabled]
    [--batch-size <value>]
    [--cli-input-json <value>]
    [--generate-cli-skeleton <value>]
<p>Example</p>

    update-event-source-mapping --uuid 12345kxodurf3443
<br>
<b>Update Function Code</b>
<p>It updates the code of the desired Lambda function</p>
<p>Syntax</p>

      update-function-code
    --function-name <value>
    [--zip-file <value>]
    [--s3-bucket <value>]
    [--s3-key <value>]
    [--s3-object-version <value>]
    [--publish | --no-publish]
    [--dry-run | --no-dry-run]
    [--cli-input-json <value>]
    [--generate-cli-skeleton <value>]
<p>Example</p>

    update-function-code --function-name functionName
<br>
<b>Update Function Configuration</b>
<p>It updates the configuration of the desired Lambda function</p>
<p>Syntax</p>

      update-function-configuration
    --function-name <value>
    [--role <value>]
    [--handler <value>]
    [--description <value>]
    [--timeout <value>]
    [--memory-size <value>]
    [--vpc-config <value>]
    [--environment <value>]
    [--runtime <value>]
    [--dead-letter-config <value>]
    [--kms-key-arn <value>]
    [--tracing-config <value>]
    [--cli-input-json <value>]
    [--generate-cli-skeleton <value>]
<p>Example</p>

    update-function-configuration --function-name functionName
<br>
<h4>References</h4>
<ul>
  <li><a href="https://aws.amazon.com/documentation/lambda/">AWS Lambda Docs</a></li>
  <li><a href="https://boto3.readthedocs.io/en/latest/">Boto 3 Docs</a></li>
  <li><a href="http://docs.aws.amazon.com/cli/latest/reference/lambda/">AWS CLI Docs</a></li>
</ul>
<br>
<b>For queries or issues, feel free to contact or open an <a href="https://github.com/srcecde/aws-lambda-cheatsheet/issues">issue</a></b>


