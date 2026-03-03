# AWS CloudFormation Cheatsheet

![CloudFormation Logo](https://a0.awsstatic.com/libra-css/images/logos/aws_logo_smile_1200x630.png)

---

## Table of Contents
1. [What is CloudFormation?](#1-what-is-cloudformation)
2. [Installation & Setup](#2-installation--setup)
3. [Template Basics](#3-template-basics)
4. [Parameters](#4-parameters)
5. [Resources](#5-resources)
6. [Outputs](#6-outputs)
7. [Functions](#7-functions)
8. [Conditions](#8-conditions)
9. [Mappings](#9-mappings)
10. [Stack Operations](#10-stack-operations)
11. [Best Practices](#11-best-practices)
12. [Troubleshooting](#12-troubleshooting)

---

## 1. What is CloudFormation?

AWS CloudFormation is an Infrastructure as Code (IaC) service that allows you to define and provision AWS infrastructure using JSON or YAML templates. It automates resource creation, configuration, and management.

**Key Features:**
- Infrastructure as Code (IaC)
- JSON and YAML templates
- Resource automation
- Stack management
- Change sets for safe updates
- Rollback on failure
- Cross-stack references
- Template parameters
- Intrinsic functions
- AWS resource support

---

## 2. Installation & Setup

### Install AWS CLI

```bash
# macOS with Homebrew
brew install awscli

# Ubuntu/Debian
sudo apt-get update
sudo apt-get install awscli

# Windows (PowerShell)
choco install awscliv2

# Direct download
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

# Verify installation
aws --version
```

### Configure AWS Credentials

```bash
# Configure credentials
aws configure

# Enter credentials when prompted:
# AWS Access Key ID: AKIAIOSFODNN7EXAMPLE
# AWS Secret Access Key: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
# Default region: us-east-1
# Default output format: json

# Verify configuration
aws sts get-caller-identity

# Use profiles
aws configure --profile prod

# List profiles
cat ~/.aws/config
```

### Project Structure

```
cloudformation-project/
├── templates/
│   ├── main.yaml                # Main template
│   ├── network.yaml             # Nested template
│   ├── compute.yaml             # Nested template
│   └── database.yaml            # Nested template
├── parameters/
│   ├── dev-params.json          # Dev parameters
│   ├── test-params.json         # Test parameters
│   └── prod-params.json         # Prod parameters
├── scripts/
│   ├── deploy.sh                # Deployment script
│   └── validate.sh              # Validation script
├── README.md
└── .gitignore
```

---

## 3. Template Basics

### Basic Template Structure

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: 'AWS CloudFormation template for web infrastructure'

Metadata:
  AWS::CloudFormation::Interface:
    ParameterGroups:
      - Label:
          default: "Network Configuration"
        Parameters:
          - VpcCIDR
          - SubnetCIDR
      - Label:
          default: "EC2 Configuration"
        Parameters:
          - InstanceType
          - KeyName

Parameters:
  # Parameter definitions

Mappings:
  # Mapping definitions

Conditions:
  # Condition definitions

Resources:
  # Resource definitions

Outputs:
  # Output definitions
```

### Minimal Template

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: 'Minimal CloudFormation template'

Resources:
  MyBucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: my-unique-bucket-name
      VersioningConfiguration:
        Status: Enabled

Outputs:
  BucketName:
    Value: !Ref MyBucket
    Description: S3 bucket name
```

### Template with All Sections

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: 'Complete CloudFormation template'

Metadata:
  AWS::CloudFormation::Interface:
    ParameterGroups:
      - Label:
          default: "Environment"
        Parameters:
          - Environment
          - InstanceType

Parameters:
  Environment:
    Type: String
    Default: dev
    AllowedValues: [dev, test, prod]
    Description: Environment name

  InstanceType:
    Type: String
    Default: t3.micro
    AllowedValues: [t3.micro, t3.small, t3.medium]

Mappings:
  RegionMap:
    us-east-1:
      AMI: ami-0c55b159cbfafe1f0
    us-west-2:
      AMI: ami-0d1cd67c26f5fca19

Conditions:
  IsProd: !Equals [!Ref Environment, prod]
  IsNotProd: !Not [!Condition IsProd]

Resources:
  MyInstance:
    Type: AWS::EC2::Instance
    Properties:
      ImageId: !FindInMap [RegionMap, !Ref 'AWS::Region', AMI]
      InstanceType: !Ref InstanceType
      Tags:
        - Key: Environment
          Value: !Ref Environment

Outputs:
  InstanceId:
    Value: !Ref MyInstance
    Description: EC2 Instance ID
  InstancePublicIP:
    Value: !GetAtt MyInstance.PublicIp
    Description: Public IP of instance
```

---

## 4. Parameters

### Parameter Types

```yaml
Parameters:
  # String parameter
  KeyName:
    Type: String
    Description: EC2 KeyPair name
    ConstraintDescription: Must be an existing KeyPair

  # Number parameter
  InstanceCount:
    Type: Number
    Default: 1
    MinValue: 1
    MaxValue: 10
    Description: Number of instances

  # List parameter
  Environment:
    Type: String
    Default: dev
    AllowedValues:
      - dev
      - test
      - prod
    Description: Environment type

  # AWS-specific parameters
  KeyPairName:
    Type: AWS::EC2::KeyPair::KeyName
    Description: EC2 KeyPair name

  # VPC parameter
  VPC:
    Type: AWS::EC2::VPC::Id
    Description: VPC ID

  # Subnet parameter
  Subnet:
    Type: AWS::EC2::Subnet::Id
    Description: Subnet ID

  # Security Group parameter
  SecurityGroup:
    Type: AWS::EC2::SecurityGroup::Id
    Description: Security group ID

  # List of values
  AvailabilityZones:
    Type: List<AWS::EC2::AvailabilityZone::Name>
    Description: List of AZs

  # List of Security Groups
  SecurityGroupIds:
    Type: List<AWS::EC2::SecurityGroup::Id>
    Description: List of security groups

  # Comma-delimited list
  TagValues:
    Type: CommaDelimitedList
    Default: "tag1,tag2,tag3"
    Description: Comma-separated tags

  # JSON parameter
  Configuration:
    Type: String
    Default: '{"key":"value"}'
    Description: JSON configuration

  # Password (masked in console)
  DBPassword:
    Type: String
    NoEcho: true
    MinLength: 8
    Description: Database password
```

### Use Parameters

```yaml
Parameters:
  InstanceType:
    Type: String
    Default: t3.micro

  Environment:
    Type: String
    Default: dev

Resources:
  MyInstance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: !Ref InstanceType
      Tags:
        - Key: Environment
          Value: !Ref Environment
        - Key: Name
          Value: !Sub '${AWS::StackName}-instance'
```

---

## 5. Resources

### Common AWS Resources

```yaml
Resources:
  # VPC
  MyVPC:
    Type: AWS::EC2::VPC
    Properties:
      CidrBlock: 10.0.0.0/16
      EnableDnsHostnames: true
      EnableDnsSupport: true
      Tags:
        - Key: Name
          Value: my-vpc

  # Subnet
  PublicSubnet:
    Type: AWS::EC2::Subnet
    Properties:
      VpcId: !Ref MyVPC
      CidrBlock: 10.0.1.0/24
      AvailabilityZone: !Select [0, !GetAZs '']
      MapPublicIpOnLaunch: true

  # Internet Gateway
  InternetGateway:
    Type: AWS::EC2::InternetGateway

  AttachGateway:
    Type: AWS::EC2::VPCGatewayAttachment
    Properties:
      VpcId: !Ref MyVPC
      InternetGatewayId: !Ref InternetGateway

  # Route Table
  PublicRouteTable:
    Type: AWS::EC2::RouteTable
    Properties:
      VpcId: !Ref MyVPC

  PublicRoute:
    Type: AWS::EC2::Route
    DependsOn: AttachGateway
    Properties:
      RouteTableId: !Ref PublicRouteTable
      DestinationCidrBlock: 0.0.0.0/0
      GatewayId: !Ref InternetGateway

  SubnetRouteTableAssociation:
    Type: AWS::EC2::SubnetRouteTableAssociation
    Properties:
      SubnetId: !Ref PublicSubnet
      RouteTableId: !Ref PublicRouteTable

  # Security Group
  WebSecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: Web server security group
      VpcId: !Ref MyVPC
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: 80
          ToPort: 80
          CidrIp: 0.0.0.0/0
        - IpProtocol: tcp
          FromPort: 443
          ToPort: 443
          CidrIp: 0.0.0.0/0
      SecurityGroupEgress:
        - IpProtocol: -1
          CidrIp: 0.0.0.0/0

  # EC2 Instance
  WebInstance:
    Type: AWS::EC2::Instance
    Properties:
      ImageId: ami-0c55b159cbfafe1f0
      InstanceType: t3.micro
      SubnetId: !Ref PublicSubnet
      SecurityGroupIds:
        - !Ref WebSecurityGroup
      UserData:
        Fn::Base64: !Sub |
          #!/bin/bash
          apt-get update
          apt-get install -y nginx
          systemctl start nginx

  # S3 Bucket
  MyBucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: !Sub '${AWS::StackName}-bucket'
      VersioningConfiguration:
        Status: Enabled
      PublicAccessBlockConfiguration:
        BlockPublicAcls: true
        BlockPublicPolicy: true
        IgnorePublicAcls: true
        RestrictPublicBuckets: true

  # RDS Instance
  MyDatabase:
    Type: AWS::RDS::DBInstance
    Properties:
      DBInstanceIdentifier: !Sub '${AWS::StackName}-db'
      Engine: postgres
      MasterUsername: postgres
      MasterUserPassword: !Sub '{{resolve:secretsmanager:${DBSecret}:SecretString:password}}'
      DBInstanceClass: db.t3.micro
      AllocatedStorage: 20

  # RDS Subnet Group
  DBSubnetGroup:
    Type: AWS::RDS::DBSubnetGroup
    Properties:
      DBSubnetGroupDescription: Subnet group for RDS
      SubnetIds:
        - !Ref PublicSubnet

  # Load Balancer
  LoadBalancer:
    Type: AWS::ElasticLoadBalancingV2::LoadBalancer
    Properties:
      Type: application
      Subnets:
        - !Ref PublicSubnet
      SecurityGroups:
        - !Ref WebSecurityGroup

  # Auto Scaling Group
  LaunchTemplate:
    Type: AWS::EC2::LaunchTemplate
    Properties:
      LaunchTemplateData:
        ImageId: ami-0c55b159cbfafe1f0
        InstanceType: t3.micro
        UserData:
          Fn::Base64: !Sub |
            #!/bin/bash
            apt-get update
            apt-get install -y nginx

  AutoScalingGroup:
    Type: AWS::AutoScaling::AutoScalingGroup
    Properties:
      LaunchTemplate:
        LaunchTemplateId: !Ref LaunchTemplate
        Version: !GetAtt LaunchTemplate.LatestVersionNumber
      MinSize: 1
      MaxSize: 5
      DesiredCapacity: 2
      VPCZoneIdentifier:
        - !Ref PublicSubnet

  # CloudWatch Alarm
  CPUAlarm:
    Type: AWS::CloudWatch::Alarm
    Properties:
      AlarmName: high-cpu
      MetricName: CPUUtilization
      Namespace: AWS/EC2
      Statistic: Average
      Period: 300
      EvaluationPeriods: 2
      Threshold: 80
      ComparisonOperator: GreaterThanThreshold

  # Secrets Manager
  DBSecret:
    Type: AWS::SecretsManager::Secret
    Properties:
      Name: !Sub '${AWS::StackName}/db/password'
      GenerateSecretString:
        SecretStringTemplate: '{"username":"admin"}'
        GenerateStringKey: password
        PasswordLength: 16

  # Lambda Function
  MyLambda:
    Type: AWS::Lambda::Function
    Properties:
      FunctionName: !Sub '${AWS::StackName}-function'
      Runtime: python3.9
      Handler: index.handler
      Code:
        ZipFile: |
          def handler(event, context):
              return {'statusCode': 200, 'body': 'Hello'}

  # SNS Topic
  NotificationTopic:
    Type: AWS::SNS::Topic
    Properties:
      TopicName: !Sub '${AWS::StackName}-topic'

  # SQS Queue
  TaskQueue:
    Type: AWS::SQS::Queue
    Properties:
      QueueName: !Sub '${AWS::StackName}-queue'
      VisibilityTimeout: 300
```

---

## 6. Outputs

### Output Declaration

```yaml
Outputs:
  # Simple output
  VpcId:
    Value: !Ref MyVPC
    Description: VPC ID

  # Output with resource attribute
  InstancePublicIP:
    Value: !GetAtt WebInstance.PublicIp
    Description: Public IP address

  # Output with substitution
  LoadBalancerDNS:
    Value: !GetAtt LoadBalancer.DNSName
    Description: Load balancer DNS name

  # Export for cross-stack reference
  DatabaseEndpoint:
    Value: !GetAtt MyDatabase.Endpoint.Address
    Description: Database endpoint
    Export:
      Name: !Sub '${AWS::StackName}-DBEndpoint'

  # Conditional output
  InstanceId:
    Value: !If [IsProd, !Ref ProdInstance, !Ref DevInstance]
    Description: Instance ID based on environment

  # Output as list
  SubnetIds:
    Value: !Join [',', [!Ref PublicSubnet, !Ref PrivateSubnet]]
    Description: Subnet IDs (comma-separated)

  # Output with condition
  HighAvailabilityDB:
    Condition: IsProd
    Value: !GetAtt MyDatabase.Endpoint.Address
    Description: Only exported in prod
```

---

## 7. Functions

### Intrinsic Functions

```yaml
Resources:
  MyResource:
    Type: AWS::S3::Bucket
    Properties:
      # Ref function - get value of parameter or resource
      BucketName: !Ref BucketName

      # GetAtt - get attribute of resource
      MyProperty: !GetAtt AnotherResource.Arn

      # Sub - substitute variables
      Key: !Sub '${AWS::StackName}-key'

      # Join - join list with delimiter
      Tags:
        - Key: Resources
          Value: !Join [',', [resource1, resource2, resource3]]

      # Select - get element from list
      AvailabilityZone: !Select [0, !GetAZs '']

      # GetAZs - get availability zones
      Zones: !GetAZs !Ref AWS::Region

      # FindInMap - get value from mappings
      ImageId: !FindInMap [RegionMap, !Ref 'AWS::Region', AMI]

      # If - conditional function
      InstanceType: !If [IsProd, t3.large, t3.micro]

      # Not - logical NOT
      EnableLogging: !Not [!Condition IsDev]

      # And - logical AND
      CreateResource: !And [!Condition IsProd, !Condition HasBudget]

      # Or - logical OR
      EnableSSL: !Or [!Condition IsProd, !Condition IsTest]

      # Equals - equality comparison
      IsProduction: !Equals [!Ref Environment, prod]

      # Base64 - encode to base64
      UserData: !Base64 |
        #!/bin/bash
        echo "Hello"

      # ImportValue - import from exported output
      VpcId: !ImportValue MyNetwork-VpcId

      # Split - split string
      SubnetList: !Split [',', '10.0.1.0/24,10.0.2.0/24']

      # GetAWSAccountId
      AccountId: !Ref 'AWS::AccountId'

      # GetNotificationARNs
      NotificationARN: !Select [0, !GetAZs '']

      # Cidr - generate CIDR blocks
      Cidr: !Cidr [10.0.0.0/16, 4, 6]
```

### Pseudo Parameters

```yaml
Resources:
  MyBucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: !Sub 'bucket-${AWS::AccountId}-${AWS::Region}'
      Tags:
        - Key: Stack
          Value: !Ref AWS::StackName
        - Key: Region
          Value: !Ref AWS::Region
        - Key: Account
          Value: !Ref AWS::AccountId

Outputs:
  PartitionName:
    Value: !Ref AWS::Partition    # aws or aws-cn

  StackName:
    Value: !Ref AWS::StackName

  StackId:
    Value: !Ref AWS::StackId

  Region:
    Value: !Ref AWS::Region

  AccountId:
    Value: !Ref AWS::AccountId

  NotificationARNs:
    Value: !Join [',', !Ref 'AWS::NotificationARNs']

  URLSuffix:
    Value: !Ref AWS::URLSuffix
```

---

## 8. Conditions

### Define Conditions

```yaml
Parameters:
  Environment:
    Type: String
    Default: dev
    AllowedValues: [dev, test, prod]

  EnableHighAvailability:
    Type: String
    Default: 'false'
    AllowedValues: ['true', 'false']

Conditions:
  # Simple condition
  IsProd: !Equals [!Ref Environment, prod]
  IsTest: !Equals [!Ref Environment, test]
  IsDev: !Equals [!Ref Environment, dev]

  # Logical NOT
  IsNotProd: !Not [!Condition IsProd]

  # Logical AND
  IsProdAndHA: !And [!Condition IsProd, !Equals [!Ref EnableHighAvailability, 'true']]

  # Logical OR
  IsTestOrProd: !Or [!Condition IsTest, !Condition IsProd]

  # Complex condition
  NeedsDatabaseHA: !And
    - !Condition IsProd
    - !Equals [!Ref EnableHighAvailability, 'true']

Resources:
  # Conditional resource creation
  ProdInstance:
    Type: AWS::EC2::Instance
    Condition: IsProd
    Properties:
      ImageId: ami-0c55b159cbfafe1f0
      InstanceType: t3.large

  DevInstance:
    Type: AWS::EC2::Instance
    Condition: IsDev
    Properties:
      ImageId: ami-0c55b159cbfafe1f0
      InstanceType: t3.micro

  # RDS with conditional properties
  Database:
    Type: AWS::RDS::DBInstance
    Properties:
      DBInstanceIdentifier: mydb
      Engine: postgres
      MasterUsername: admin
      MasterUserPassword: password
      DBInstanceClass: !If [IsProd, db.t3.large, db.t3.micro]
      AllocatedStorage: !If [IsProd, 100, 20]
      MultiAZ: !If [IsProd, true, false]

Outputs:
  InstanceType:
    Condition: IsProd
    Value: !Ref ProdInstance
    Description: Production instance
```

---

## 9. Mappings

### Define Mappings

```yaml
Mappings:
  # Region to AMI mapping
  RegionMap:
    us-east-1:
      AMI: ami-0c55b159cbfafe1f0
      InstanceType: t3.micro
    us-west-2:
      AMI: ami-0d1cd67c26f5fca19
      InstanceType: t3.small
    eu-west-1:
      AMI: ami-0713f98de93659029
      InstanceType: t3.micro

  # Environment mapping
  EnvironmentConfig:
    dev:
      InstanceType: t3.micro
      DBSize: 10
      MultiAZ: false
    test:
      InstanceType: t3.small
      DBSize: 20
      MultiAZ: false
    prod:
      InstanceType: t3.large
      DBSize: 100
      MultiAZ: true

  # Architecture mapping
  ArchMap:
    x86_64:
      bits: '64'
      name: 'x86 64-bit'
    i386:
      bits: '32'
      name: 'x86 32-bit'

Resources:
  MyInstance:
    Type: AWS::EC2::Instance
    Properties:
      # Use mapping
      ImageId: !FindInMap [RegionMap, !Ref 'AWS::Region', AMI]
      InstanceType: !FindInMap [EnvironmentConfig, !Ref Environment, InstanceType]

  MyDatabase:
    Type: AWS::RDS::DBInstance
    Properties:
      DBInstanceClass: db.t3.micro
      AllocatedStorage: !FindInMap [EnvironmentConfig, !Ref Environment, DBSize]
      MultiAZ: !FindInMap [EnvironmentConfig, !Ref Environment, MultiAZ]
```

---

## 10. Stack Operations

### Create Stack

```bash
# Create stack from template
aws cloudformation create-stack \
  --stack-name my-stack \
  --template-body file://template.yaml

# Create with parameters
aws cloudformation create-stack \
  --stack-name my-stack \
  --template-body file://template.yaml \
  --parameters ParameterKey=Environment,ParameterValue=prod \
               ParameterKey=InstanceType,ParameterValue=t3.large

# Create with parameter file
aws cloudformation create-stack \
  --stack-name my-stack \
  --template-body file://template.yaml \
  --parameters file://parameters.json

# Create with capabilities
aws cloudformation create-stack \
  --stack-name my-stack \
  --template-body file://template.yaml \
  --capabilities CAPABILITY_IAM CAPABILITY_NAMED_IAM

# Create with tags
aws cloudformation create-stack \
  --stack-name my-stack \
  --template-body file://template.yaml \
  --tags Key=Environment,Value=prod \
         Key=Owner,Value=teamA
```

### Update Stack

```bash
# Update stack
aws cloudformation update-stack \
  --stack-name my-stack \
  --template-body file://template.yaml \
  --parameters ParameterKey=InstanceType,ParameterValue=t3.medium

# Create change set (preview changes)
aws cloudformation create-change-set \
  --stack-name my-stack \
  --change-set-name my-changeset \
  --template-body file://template.yaml \
  --parameters ParameterKey=InstanceType,ParameterValue=t3.medium \
  --change-set-type UPDATE

# View change set
aws cloudformation describe-change-set \
  --stack-name my-stack \
  --change-set-name my-changeset

# Execute change set
aws cloudformation execute-change-set \
  --stack-name my-stack \
  --change-set-name my-changeset

# Delete change set
aws cloudformation delete-change-set \
  --stack-name my-stack \
  --change-set-name my-changeset
```

### Manage Stack

```bash
# Describe stack
aws cloudformation describe-stacks --stack-name my-stack

# List stacks
aws cloudformation list-stacks

# List stack resources
aws cloudformation list-stack-resources --stack-name my-stack

# Describe stack resources
aws cloudformation describe-stack-resources --stack-name my-stack

# Get stack events
aws cloudformation describe-stack-events --stack-name my-stack

# Validate template
aws cloudformation validate-template --template-body file://template.yaml

# Delete stack
aws cloudformation delete-stack --stack-name my-stack

# Wait for stack completion
aws cloudformation wait stack-create-complete --stack-name my-stack
```

### Parameter Files

```json
// parameters.json
[
  {
    "ParameterKey": "Environment",
    "ParameterValue": "prod"
  },
  {
    "ParameterKey": "InstanceType",
    "ParameterValue": "t3.large"
  },
  {
    "ParameterKey": "DBPassword",
    "ParameterValue": "SecurePassword123!"
  }
]
```

---

## 11. Best Practices

### Naming Conventions

```yaml
Parameters:
  EnvironmentName:
    Type: String
    Description: Environment name (dev/test/prod)

  ApplicationName:
    Type: String
    Description: Application name

Resources:
  # Use consistent naming
  MyBucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: !Sub '${ApplicationName}-${EnvironmentName}-${AWS::AccountId}'

  MySecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupName: !Sub '${ApplicationName}-sg-${EnvironmentName}'

Outputs:
  BucketName:
    Value: !Ref MyBucket
    Export:
      Name: !Sub '${AWS::StackName}-BucketName'
```

### Modular Templates

```yaml
# main.yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: 'Main template'

Parameters:
  EnvironmentName:
    Type: String

Resources:
  # Network Stack
  NetworkStack:
    Type: AWS::CloudFormation::Stack
    Properties:
      StackName: !Sub '${AWS::StackName}-network'
      TemplateURL: https://s3.amazonaws.com/bucket/network.yaml
      Parameters:
        EnvironmentName: !Ref EnvironmentName

  # Compute Stack
  ComputeStack:
    Type: AWS::CloudFormation::Stack
    Properties:
      StackName: !Sub '${AWS::StackName}-compute'
      TemplateURL: https://s3.amazonaws.com/bucket/compute.yaml
      Parameters:
        VpcId: !GetAtt NetworkStack.Outputs.VpcId
        EnvironmentName: !Ref EnvironmentName

Outputs:
  VpcId:
    Value: !GetAtt NetworkStack.Outputs.VpcId
  InstanceId:
    Value: !GetAtt ComputeStack.Outputs.InstanceId
```

### Idempotent Resources

```yaml
Resources:
  # Good - uses logical names
  MyBucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: !Sub 'my-app-${AWS::AccountId}-${AWS::Region}'

  # Good - conditional creation
  ProdResources:
    Type: AWS::S3::Bucket
    Condition: IsProd
    Properties:
      BucketName: !Sub 'prod-bucket-${AWS::AccountId}'

  # Bad - timestamp changes each run
  DynamicBucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: !Sub 'bucket-${AWS::StackName}-${AWS::StackId}'
```

### Tag Strategy

```yaml
Parameters:
  Environment:
    Type: String
  Owner:
    Type: String
  CostCenter:
    Type: String

Mappings:
  TagMap:
    dev:
      BackupPolicy: weekly
    prod:
      BackupPolicy: daily

Resources:
  MyBucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: my-bucket
      Tags:
        - Key: Environment
          Value: !Ref Environment
        - Key: Owner
          Value: !Ref Owner
        - Key: CostCenter
          Value: !Ref CostCenter
        - Key: ManagedBy
          Value: CloudFormation
        - Key: BackupPolicy
          Value: !FindInMap [TagMap, !Ref Environment, BackupPolicy]
```

---

## 12. Troubleshooting

### Validate Template

```bash
# Validate syntax
aws cloudformation validate-template \
  --template-body file://template.yaml

# Lint with cfn-lint
cfn-lint template.yaml

# Check style with cfn-python-lint
python -m cfn_lint template.yaml

# Validate with specific region
aws cloudformation validate-template \
  --region us-east-1 \
  --template-body file://template.yaml
```

### Common Issues

```yaml
# Issue: Missing required property
# Wrong:
MyInstance:
  Type: AWS::EC2::Instance
  Properties:
    InstanceType: t3.micro

# Right:
MyInstance:
  Type: AWS::EC2::Instance
  Properties:
    ImageId: ami-0c55b159cbfafe1f0
    InstanceType: t3.micro

# Issue: Circular dependency
# Wrong - circular reference
ResourceA:
  DependsOn: ResourceB
ResourceB:
  DependsOn: ResourceA

# Right - use !Ref implicitly
ResourceA:
  Properties:
    SecurityGroupId: !Ref ResourceB

# Issue: Invalid reference
# Wrong:
MyProperty: !Ref MySecurityGroup.GroupId

# Right:
MyProperty: !GetAtt MySecurityGroup.GroupId

# Issue: Export name conflict
# Wrong - same export name in multiple stacks
Outputs:
  VpcId:
    Export:
      Name: VpcId

# Right - unique export names
Outputs:
  VpcId:
    Export:
      Name: !Sub '${AWS::StackName}-VpcId'
```

### Debugging Stack

```bash
# Get stack events (real-time logs)
aws cloudformation describe-stack-events \
  --stack-name my-stack \
  --query 'StackEvents[*].[LogicalResourceId,ResourceStatus,ResourceStatusReason]' \
  --output table

# Watch stack creation
watch -n 5 'aws cloudformation describe-stacks \
  --stack-name my-stack \
  --query "Stacks[0].StackStatus"'

# Get failed resource details
aws cloudformation describe-stack-resources \
  --stack-name my-stack \
  --query 'StackResources[?ResourceStatus==`CREATE_FAILED`]'

# View outputs
aws cloudformation describe-stacks \
  --stack-name my-stack \
  --query 'Stacks[0].Outputs'

# Get specific property
aws cloudformation describe-stack-resource \
  --stack-name my-stack \
  --logical-resource-id MyBucket
```

### Rollback

```bash
# Set rollback on failure
aws cloudformation create-stack \
  --stack-name my-stack \
  --template-body file://template.yaml \
  --on-failure ROLLBACK

# Disable rollback for debugging
aws cloudformation create-stack \
  --stack-name my-stack \
  --template-body file://template.yaml \
  --on-failure DO_NOTHING

# Continue update on failure
aws cloudformation update-stack \
  --stack-name my-stack \
  --template-body file://template.yaml \
  --disable-rollback

# Signal success (for WaitCondition)
/opt/aws/bin/cfn-signal -e $? --stack my-stack \
  --resource MyWaitCondition --region us-east-1
```

---

## Quick Reference

```bash
# Template Operations
aws cloudformation validate-template --template-body file://template.yaml
aws cloudformation create-stack --stack-name my-stack --template-body file://template.yaml
aws cloudformation update-stack --stack-name my-stack --template-body file://template.yaml
aws cloudformation delete-stack --stack-name my-stack

# Parameters
aws cloudformation create-stack --stack-name my-stack \
  --template-body file://template.yaml \
  --parameters ParameterKey=Env,ParameterValue=prod

# Change Sets
aws cloudformation create-change-set --stack-name my-stack \
  --change-set-name my-changeset --template-body file://template.yaml
aws cloudformation execute-change-set --stack-name my-stack \
  --change-set-name my-changeset

# Stack Info
aws cloudformation describe-stacks --stack-name my-stack
aws cloudformation list-stack-resources --stack-name my-stack
aws cloudformation describe-stack-events --stack-name my-stack

# Validation
cfn-lint template.yaml
aws cloudformation validate-template --template-body file://template.yaml
```

---

## Resources

| Resource | URL |
|----------|-----|
| **Official Documentation** | https://docs.aws.amazon.com/cloudformation/ |
| **User Guide** | https://docs.aws.amazon.com/cloudformation/latest/userguide/ |
| **Resource Reference** | https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html |
| **Best Practices** | https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/best-practices.html |
| **cfn-lint** | https://github.com/aws-cloudformation/cfn-lint |
| **AWS CLI** | https://docs.aws.amazon.com/cli/latest/reference/cloudformation/ |

---

**Last Updated:** 2024
**CloudFormation Version:** Latest
**AWS CLI Version:** 2.x+

For latest information, visit [AWS CloudFormation Documentation](https://docs.aws.amazon.com/cloudformation/)