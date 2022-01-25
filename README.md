## On the Topics page, choose Create topic.

### On the Create topic page, in the Details section, do the following:

  - For Type, choose a topic type `Standard`
  - Enter a Name for the topic. exp. `ec2-sns-eventbridge`
  - Enter a Display name for the topic. `ec2-sns-eventbridge`

    <p align="center">
    <img src="./images/img1.png" />
    </p>

  - Then Click On create

###  Subscribing to an Amazon SNS topic
  - select topic  `ec2-sns-eventbridge` and click on `create subscription`

    <p align="center">
    <img src="./images/img2.png" />
    </p>
  - On the Create subscription page, in the Details section, do the following:
     - For Topic ARN, choose the Amazon Resource Name (ARN) of a topic.
     - For Protocol, choose an endpoint type. `Email` 
    <br> 
    <p align="center">
    <img src="./images/img3.png" />
    </p>

  - Choose Create subscription.

<br><br>

## Create Lambda Function  :

  - Open the Functions page of the Lambda console.
  - Choose Create function.

  - Under Basic information, do the following:

        For Function name, enter `ec2-sns-eventbridge`.

        For Runtime, select `python3.X`


    <p align="center">
    <img src="./images/img4.png" />
    </p>

  - In Code source section :
   
    ```python

    import json
    import boto3

    def lambda_handler(event, context):
        try:
            print("Event =====>",event)
            notification = "New Instance Lanunched Details : \n" + "ACCOUNT =" + str(event['account']) + " \n Instance ID = "+ str(event['detail']['responseElements']['instancesSet']['items'][0]['instanceId'])
            client = boto3.client('sns')
            response = client.publish(
                TargetArn="arn:aws:sns:ap-south-1:828232558101:ec2-sns-eventbridge",
                Message=json.dumps({'default': notification}),
                MessageStructure='json')
        except Exception as ex:
            response = json.dumps({'default': "Error Occured In Running Lambda"+str(ex)})

        return {
            'statusCode': 200,
            'body': json.dumps(response)
        }
        
     
    ```

  - Then click on deploy

  # Complete JSON DATA (Parsed InstanceID and Account No for Email In above Code ) :

  ```JSON
        {'version': '0', 'id': '71e2f148-a626-4e54-93b5-a08e13e1410c', 'detail-type': 'AWS API Call via CloudTrail', 'source': 'aws.ec2', 'account': '828232558101', 'time': '2022-01-24T17:42:24Z', 'region': 'ap-south-1', 'resources': [], 'detail': {'eventVersion': '1.08', 'userIdentity': {'type': 'Root', 'principalId': '828232558101', 'arn': 'arn:aws:iam::828232558101:root', 'accountId': '828232558101', 'accessKeyId': 'ASIA4BVUB4YKWHQHPB75', 'sessionContext': {'sessionIssuer': {}, 'webIdFederationData': {}, 'attributes': {'creationDate': '2022-01-24T16:45:59Z', 'mfaAuthenticated': 'false'}}}, 'eventTime': '2022-01-24T17:42:24Z', 'eventSource': 'ec2.amazonaws.com', 'eventName': 'RunInstances', 'awsRegion': 'ap-south-1', 'sourceIPAddress': '103.163.91.18', 'userAgent': 'console.ec2.amazonaws.com', 'requestParameters': {'instancesSet': {'items': [{'imageId': 'ami-0af25d0df86db00c1', 'minCount': 1, 'maxCount': 1, 'keyName': 'abcdsjdaskda'}]}, 'groupSet': {'items': [{'groupId': 'sg-07fc7f6f188a494c6'}]}, 'instanceType': 't2.micro', 'blockDeviceMapping': {'items': [{'deviceName': '/dev/xvda', 'ebs': {'volumeSize': 8, 'deleteOnTermination': True, 'volumeType': 'gp2'}}]}, 'tenancy': 'default', 'monitoring': {'enabled': False}, 'disableApiTermination': False, 'disableApiStop': False, 'instanceInitiatedShutdownBehavior': 'stop', 'ebsOptimized': False, 'creditSpecification': {'cpuCredits': 'standard'}, 'capacityReservationSpecification': {'capacityReservationPreference': 'open'}, 'hibernationOptions': {'configured': False}, 'metadataOptions': {'httpTokens': 'optional', 'httpPutResponseHopLimit': 1, 'httpEndpoint': 'enabled', 'instanceMetadataTags': 'enabled'}, 'privateDnsNameOptions': {'hostnameType': 'ip-name', 'enableResourceNameDnsARecord': True, 'enableResourceNameDnsAAAARecord': False}}, 'responseElements': {'requestId': 'a2c95fe1-f98a-4b25-bec5-5e089e62a9d7', 'reservationId': 'r-044298d9454037cc5', 'ownerId': '828232558101', 'groupSet': {}, 'instancesSet': {'items': [{'instanceId': 'i-0f0bc4d6f65bfe380', 'imageId': 'ami-0af25d0df86db00c1', 'instanceState': {'code': 0, 'name': 'pending'}, 'privateDnsName': 'ip-172-31-10-249.ap-south-1.compute.internal', 'keyName': 'abcdsjdaskda', 'amiLaunchIndex': 0, 'productCodes': {}, 'instanceType': 't2.micro', 'launchTime': 1643046144000, 'placement': {'availabilityZone': 'ap-south-1b', 'tenancy': 'default'}, 'monitoring': {'state': 'disabled'}, 'subnetId': 'subnet-0ce3d6ded701497f0', 'vpcId': 'vpc-0b63813368bba017f', 'privateIpAddress': '172.31.10.249', 'stateReason': {'code': 'pending', 'message': 'pending'}, 'architecture': 'x86_64', 'rootDeviceType': 'ebs', 'rootDeviceName': '/dev/xvda', 'blockDeviceMapping': {}, 'virtualizationType': 'hvm', 'hypervisor': 'xen', 'groupSet': {'items': [{'groupId': 'sg-07fc7f6f188a494c6', 'groupName': 'launch-wizard-20'}]}, 'sourceDestCheck': True, 'networkInterfaceSet': {'items': [{'networkInterfaceId': 'eni-0d9266ad27a410021', 'subnetId': 'subnet-0ce3d6ded701497f0', 'vpcId': 'vpc-0b63813368bba017f', 'ownerId': '828232558101', 'status': 'in-use', 'macAddress': '0a:d7:b2:71:be:96', 'privateIpAddress': '172.31.10.249', 'privateDnsName': 'ip-172-31-10-249.ap-south-1.compute.internal', 'sourceDestCheck': True, 'interfaceType': 'interface', 'groupSet': {'items': [{'groupId': 'sg-07fc7f6f188a494c6', 'groupName': 'launch-wizard-20'}]}, 'attachment': {'attachmentId': 'eni-attach-077b73bfa8b2de50d', 'deviceIndex': 0, 'networkCardIndex': 0, 'status': 'attaching', 'attachTime': 1643046144000, 'deleteOnTermination': True}, 'privateIpAddressesSet': {'item': [{'privateIpAddress': '172.31.10.249', 'privateDnsName': 'ip-172-31-10-249.ap-south-1.compute.internal', 'primary': True}]}, 'ipv6AddressesSet': {}, 'tagSet': {}}]}, 'ebsOptimized': False, 'enaSupport': True, 'cpuOptions': {'coreCount': 1, 'threadsPerCore': 1}, 'capacityReservationSpecification': {'capacityReservationPreference': 'open'}, 'hibernationOptions': {'configured': False}, 'enclaveOptions': {'enabled': False}, 'metadataOptions': {'state': 'pending', 'httpTokens': 'optional', 'httpPutResponseHopLimit': 1, 'httpEndpoint': 'enabled', 'httpProtocolIpv4': 'enabled', 'httpProtocolIpv6': 'disabled', 'instanceMetadataTags': 'enabled'}, 'privateDnsNameOptions': {'hostnameType': 'ip-name', 'enableResourceNameDnsARecord': True, 'enableResourceNameDnsAAAARecord': False}}]}}, 'requestID': 'a2c95fe1-f98a-4b25-bec5-5e089e62a9d7', 'eventID': '06b9ee0a-715d-4421-8142-f03ca2f7675e', 'readOnly': False, 'eventType': 'AwsApiCall', 'managementEvent': True, 'recipientAccountId': '828232558101', 'eventCategory': 'Management'}}
  ```

<br><br>

## create a rule for an AWS service 
  - Open the Amazon EventBridge console 
  - In the navigation pane, choose Rules.
  - Choose Create rule.
  - Enter a name and description for the rule.
  - For Define pattern, choose `Event pattern.`
  - paste 
    ```JSON
        {
        "source": [
            "aws.ec2"
        ],
        "detail-type": [
            "AWS API Call via CloudTrail"
        ],
        "detail": {
            "eventSource": [
            "ec2.amazonaws.com"
            ],
            "eventName": [
            "RunInstances"
            ]
        }
        }
    ```

    <p align="center">
    <img src="./images/img5.png" />
    </p>

    - Then click on save button 

    - Now scroll down and  Select targets

    - target = Lambda function ie `ec2-sns-eventbridge`

    - And click on create 

    <p align="center">
    <img src="./images/img6.png" />
    </p>

## Updating eventbridge role 

   - Go to aws iam roles console 
   - search for eventbridge role for you newly created eventbridge

<p align="center">
<img src="./images/img7.png" />
</p>

   - Update polic 
      * Ec2FullAccess
      * LambdaFull Access
      * SNS FUll Access

<p align="center">
<img src="./images/img8.png" />
</p>

# Now You Can Test Your Env 