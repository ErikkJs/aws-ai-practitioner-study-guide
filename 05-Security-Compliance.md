# Domain 5: Security, Compliance, and Governance (14%)

[← Back to README](00-README.md) | [← Previous: Domain 4](04-Responsible-AI.md)

---

## AWS Shared Responsibility Model

- **AWS responsibility** (security OF the cloud): infrastructure, hardware, software, networking, facilities
- **Customer responsibility** (security IN the cloud): data management, access controls, application security, guardrails configuration
- For Bedrock: customers manage data, access controls, and guardrails

```
  SHARED RESPONSIBILITY MODEL

  ┌───────────────────────────────────────────────────────────┐
  │                 CUSTOMER RESPONSIBILITY                    │
  │              (Security IN the Cloud)                       │
  │                                                            │
  │  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐  │
  │  │ Your     │  │ Access   │  │ Guardrail│  │ Encryption│  │
  │  │ Data     │  │ Controls │  │ Config   │  │ Settings  │  │
  │  │          │  │ (IAM)    │  │          │  │ (KMS keys)│  │
  │  └──────────┘  └──────────┘  └──────────┘  └──────────┘  │
  ├───────────────────────────────────────────────────────────┤
  │                   AWS RESPONSIBILITY                       │
  │              (Security OF the Cloud)                       │
  │                                                            │
  │  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐  │
  │  │ Physical │  │ Network  │  │ Hardware │  │ Software  │  │
  │  │ Security │  │ Infra    │  │ Servers  │  │ Patching  │  │
  │  │(data ctr)│  │          │  │          │  │           │  │
  │  └──────────┘  └──────────┘  └──────────┘  └──────────┘  │
  └───────────────────────────────────────────────────────────┘

  Simple way to remember:
  - If you CAN configure it in the console → YOUR responsibility
  - If you CAN'T touch it → AWS responsibility
```

---

## Security Services and Concepts

| Service/Concept | Purpose |
|---|---|
| **AWS IAM** | Identity and access management; **least privilege principle** |
| **AWS KMS** | Key Management Service — **encryption at rest and in transit** |
| **AWS Macie** | Discover and protect **PII and sensitive data** in S3 |
| **AWS PrivateLink** | **Private connectivity** between VPCs — data never traverses public internet |
| **VPC Endpoints** | Access AWS services privately from within a VPC |
| **Amazon GuardDuty** | Intelligent **threat detection** — monitors for malicious activities |
| **Amazon Inspector** | Automated **security assessment** — identifies vulnerabilities |

### AWS CLI: Create a KMS Encryption Key

```bash
# Create a KMS key for encrypting your AI model data
aws kms create-key \
    --description "Key for Bedrock data encryption" \
    --key-usage ENCRYPT_DECRYPT \
    --key-spec SYMMETRIC_DEFAULT

# Create an alias (human-friendly name) for the key
aws kms create-alias \
    --alias-name alias/bedrock-data-key \
    --target-key-id <key-id-from-above>

# Encrypt a file with the key
aws kms encrypt \
    --key-id alias/bedrock-data-key \
    --plaintext fileb://training-data.json \
    --output text \
    --query CiphertextBlob > encrypted-data.txt

# Decrypt the file
aws kms decrypt \
    --ciphertext-blob fileb://encrypted-data.txt \
    --output text \
    --query Plaintext | base64 --decode > decrypted-data.json
```

```
  DATA FLOW WITH ENCRYPTION

  ┌──────────┐        ┌─────────────┐        ┌──────────┐
  │ Your App │──TLS──▶│   Amazon    │──TLS──▶│ Bedrock  │
  │          │Encrypted│  VPC/       │Encrypted│ FM       │
  │          │in transit│ PrivateLink│in transit│          │
  └──────────┘        └─────────────┘        └──────────┘
                             │
                             ▼
                      ┌─────────────┐
                      │    S3       │ ← Encrypted at rest
                      │  (data)    │    with KMS key
                      └─────────────┘

  TWO types of encryption:
  • In Transit = data moving between services (TLS/SSL)
  • At Rest = data sitting in storage (KMS encryption)
  Both are required for security compliance!
```

### AWS CLI: Set Up IAM Role with Least Privilege

```bash
# Create a role specifically for Bedrock access
aws iam create-role \
    --role-name BedrockTeamARole \
    --assume-role-policy-document '{
        "Version": "2012-10-17",
        "Statement": [{
            "Effect": "Allow",
            "Principal": {"Service": "bedrock.amazonaws.com"},
            "Action": "sts:AssumeRole"
        }]
    }'

# Attach a policy that only allows invoking specific models
aws iam put-role-policy \
    --role-name BedrockTeamARole \
    --policy-name BedrockInvokeOnly \
    --policy-document '{
        "Version": "2012-10-17",
        "Statement": [{
            "Effect": "Allow",
            "Action": [
                "bedrock:InvokeModel",
                "bedrock:InvokeModelWithResponseStream"
            ],
            "Resource": "arn:aws:bedrock:us-east-1::foundation-model/anthropic.claude-3-haiku-*"
        }]
    }'
```

---

## Compliance Services

| Service | Purpose |
|---|---|
| **AWS Config** | Continuous monitoring and recording of **resource configurations**; automated compliance checking |
| **AWS Artifact** | On-demand access to **compliance reports and agreements**; email notifications for new reports; includes ISV compliance reports |
| **AWS Audit Manager** | Automates **evidence collection** for auditing; assesses AWS environment against compliance frameworks |
| **AWS CloudTrail** | **API activity and audit logging** — tracks who did what, when |
| **AWS Trusted Advisor** | Guidance to optimize resources (security, cost, performance); NOT for compliance reports |

### When to Use Each Compliance Service (Scenario-Based)

| Scenario | Use This Service |
|---|---|
| "An auditor needs to see our SOC 2 report" | **AWS Artifact** |
| "We need proof that our resources meet HIPAA standards" | **AWS Audit Manager** |
| "Who deleted the S3 bucket last Tuesday?" | **AWS CloudTrail** |
| "Is our EC2 instance configured with encryption enabled?" | **AWS Config** |
| "Are we following AWS security best practices?" | **AWS Trusted Advisor** |
| "We need to continuously check that all resources have encryption on" | **AWS Config** (rules) |
| "Prepare evidence for our annual audit" | **AWS Audit Manager** |

### AWS Config vs. Audit Manager vs. Artifact vs. CloudTrail

| Config | Audit Manager | Artifact | CloudTrail |
|---|---|---|---|
| Resource **configurations** | **Audit** evidence collection | Compliance **reports** | **API activity** logging |
| "Are resources configured correctly?" | "Prove compliance" | "Show me the reports" | "Who did what?" |

---

## JSON: IAM Policy for Bedrock Access (Least Privilege)

```json
// IAM Policy: Allow Team A to invoke Bedrock models
// and access ONLY their team's S3 data
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowBedrockInvoke",
            "Effect": "Allow",
            "Action": [
                "bedrock:InvokeModel",
                "bedrock:InvokeModelWithResponseStream"
            ],
            "Resource": [
                "arn:aws:bedrock:us-east-1::foundation-model/anthropic.claude-*",
                "arn:aws:bedrock:us-east-1::foundation-model/amazon.titan-*"
            ]
        },
        {
            "Sid": "AllowTeamAS3Only",
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:PutObject",
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::company-data/team-a/*",
                "arn:aws:s3:::company-data"
            ],
            "Condition": {
                "StringLike": {
                    "s3:prefix": ["team-a/*"]
                }
            }
        }
    ]
}
```

```json
// S3 Bucket Policy: Restrict access per team
// Each team can only access their own folder
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "TeamAAccessOnly",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::123456789012:role/BedrockTeamARole"
            },
            "Action": ["s3:GetObject", "s3:PutObject"],
            "Resource": "arn:aws:s3:::company-data/team-a/*"
        },
        {
            "Sid": "TeamBAccessOnly",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::123456789012:role/BedrockTeamBRole"
            },
            "Action": ["s3:GetObject", "s3:PutObject"],
            "Resource": "arn:aws:s3:::company-data/team-b/*"
        }
    ]
}
```

> **Trap:** The exam loves to test this pattern. When a question says
> "each team should only access their own data," the answer involves
> **separate IAM roles per team** with S3 policies scoped to their prefix.
> NEVER create a single role with full S3 access.

---

## CloudFormation: VPC Endpoint for Bedrock (PrivateLink)

```yaml
# CloudFormation template: Create a VPC Endpoint for Amazon Bedrock
# This ensures traffic between your VPC and Bedrock stays on AWS's private network
# (never crosses the public internet)

AWSTemplateFormatVersion: "2010-09-09"
Description: VPC Endpoint for Amazon Bedrock using PrivateLink

Parameters:
  VpcId:
    Type: AWS::EC2::VPC::Id
    Description: The VPC where the endpoint will be created
  SubnetIds:
    Type: List<AWS::EC2::Subnet::Id>
    Description: Subnets for the endpoint network interfaces

Resources:
  # Security group to control who can talk to the endpoint
  BedrockEndpointSG:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: Allow HTTPS access to Bedrock endpoint
      VpcId: !Ref VpcId
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: 443
          ToPort: 443
          CidrIp: 10.0.0.0/16  # Only your VPC CIDR

  # The VPC Endpoint itself
  BedrockVpcEndpoint:
    Type: AWS::EC2::VPCEndpoint
    Properties:
      ServiceName: !Sub "com.amazonaws.${AWS::Region}.bedrock-runtime"
      VpcEndpointType: Interface
      VpcId: !Ref VpcId
      SubnetIds: !Ref SubnetIds
      SecurityGroupIds:
        - !Ref BedrockEndpointSG
      PrivateDnsEnabled: true

Outputs:
  EndpointId:
    Value: !Ref BedrockVpcEndpoint
    Description: Bedrock VPC Endpoint ID
```

```bash
# Deploy the VPC Endpoint
aws cloudformation deploy \
    --template-file bedrock-vpc-endpoint.yaml \
    --stack-name bedrock-private-access \
    --parameter-overrides \
        VpcId=vpc-12345678 \
        SubnetIds=subnet-aaaa,subnet-bbbb
```

---

## Compliance Standards

| Standard | Description |
|---|---|
| **ISO/IEC 27001** | Information security management system |
| **ISO/IEC 42001** | **AI management system** — AWS was first major cloud provider certified |
| **SOC 1, SOC 2, SOC 3** | Service Organization Controls — security, availability, processing integrity |
| **HIPAA** | Healthcare data protection (US) |
| **GDPR** | Data protection regulation (EU) |

---

## Data Governance Concepts

| Concept | Definition |
|---|---|
| **Data Residency** | Where data is **physically stored** (geographic location) — important for regulatory compliance |
| **Data Logging** | Tracks **data access and changes** over time (audit trail) |
| **Data Lineage** | Tracks **origin, movement, and transformations** of data throughout its lifecycle |
| **Data Cataloging** | Organizing metadata about data assets (AWS Glue Data Catalog) |
| **Data Lifecycle Policies** | Rules for data creation, retention, archival, and deletion |
| **Retention Policies** | How long data is kept before deletion |

---

## IAM and Access Control for Amazon Bedrock
- Create **separate service roles** for each team
- Grant access only to specific team's data in S3
- Fine-grained control at the team level
- Do NOT create a single role with full S3 access
- **Least privilege principle** — only grant permissions needed for the task
- Proactive access management > reactive monitoring

---

## Cloud Deployment Models

| Model | Description |
|---|---|
| **Cloud** | Fully deployed in the cloud |
| **Hybrid** | Combination of on-premises + cloud |
| **Private (On-premises)** | Resources deployed on-premises using virtualization |

## Regional vs. Global AWS Services

| Regional Services | Global Services |
|---|---|
| AWS Lambda | AWS IAM |
| Amazon Rekognition | Amazon CloudFront |
| Amazon S3 (global namespace, regional buckets) | Amazon Route 53 |
| Most AWS services | AWS WAF |

---

## Security Checklist for Deploying AI on AWS

- [ ] **IAM**: Create separate roles per team with least privilege
- [ ] **Encryption at Rest**: Enable KMS encryption for S3, SageMaker, Bedrock data
- [ ] **Encryption in Transit**: Ensure TLS/SSL for all data transfers
- [ ] **PrivateLink**: Set up VPC endpoints for Bedrock to avoid public internet
- [ ] **Data Access**: Scope S3 bucket policies to team-specific prefixes
- [ ] **Logging**: Enable CloudTrail for API activity auditing
- [ ] **Config Rules**: Set up AWS Config rules to check encryption and access settings
- [ ] **PII Protection**: Enable Macie on S3 buckets containing customer data
- [ ] **Threat Detection**: Enable GuardDuty for your account
- [ ] **Guardrails**: Configure Bedrock Guardrails for content filtering and PII redaction
- [ ] **Compliance**: Use Audit Manager frameworks for your industry (HIPAA, SOC 2, etc.)
- [ ] **Review**: Schedule periodic access reviews and policy audits

---

[Next: AWS Services Cheat Sheet →](06-AWS-Services-Cheat-Sheet.md)
