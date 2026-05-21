# AWS CDK

[AWS CDK Documentation](https://docs.aws.amazon.com/cdk/v2/guide/) | [CDK API Reference](https://docs.aws.amazon.com/cdk/api/v2/)

The AWS Cloud Development Kit (CDK) lets you define AWS infrastructure using real programming languages (TypeScript, Python, Java, Kotlin, C#) instead of YAML/JSON configuration files. CDK compiles your code into CloudFormation templates and deploys them.

---

## The Problem with Raw CloudFormation

CloudFormation is AWS's native infrastructure-as-code format — but it's verbose YAML or JSON:

```yaml
# CloudFormation: 15 lines for a simple S3 bucket
Resources:
  KwizzFilesBucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: kwizz-files
      VersioningConfiguration:
        Status: Enabled
      BucketEncryption:
        ServerSideEncryptionConfiguration:
          - ServerSideEncryptionByDefault:
              SSEAlgorithm: AES256
```

CDK equivalent in TypeScript:

```typescript
const bucket = new s3.Bucket(this, 'KwizzFiles', {
  versioned: true,
  encryption: s3.BucketEncryption.S3_MANAGED,
});
```

---

## How CDK Works

```
CDK App (TypeScript / Python / Java)
        ↓  cdk synth
CloudFormation Template (generated)
        ↓  cdk deploy
AWS Resources (created / updated)
```

1. Write infrastructure as code in your chosen language
2. `cdk synth` — generates CloudFormation templates (no deployment)
3. `cdk deploy` — deploys the generated templates to your AWS account

---

## CDK Advantages Over Raw CloudFormation

| Raw CloudFormation | CDK |
|---|---|
| YAML / JSON only | Any supported programming language |
| Copy-paste to reuse | Functions, loops, classes, modules |
| No type safety | Full IDE autocomplete and type checking |
| Hard to test | Can write unit tests for infrastructure |
| Verbose for complex setups | Constructs abstract common patterns |

---

## Constructs

The core CDK abstraction is the **Construct** — a reusable infrastructure component. CDK provides three levels:

| Level | Description | Example |
|---|---|---|
| **L1** | Direct CloudFormation resource mapping | `CfnBucket` |
| **L2** | Higher-level abstraction with sensible defaults | `Bucket` |
| **L3 (Patterns)** | Pre-built combinations of multiple resources | `ApplicationLoadBalancedFargateService` |

L2 constructs are the most commonly used — they hide boilerplate while remaining flexible.

---

## Example: Lambda + SQS

```typescript
import * as cdk from 'aws-cdk-lib';
import * as lambda from 'aws-cdk-lib/aws-lambda';
import * as sqs from 'aws-cdk-lib/aws-sqs';
import * as lambdaEventSources from 'aws-cdk-lib/aws-lambda-event-sources';

const queue = new sqs.Queue(this, 'KwizzJobQueue', {
  visibilityTimeout: cdk.Duration.seconds(30),
});

const processor = new lambda.Function(this, 'JobProcessor', {
  runtime: lambda.Runtime.PROVIDED_AL2,
  handler: 'handler',
  code: lambda.Code.fromAsset('build/lambda.zip'),
});

processor.addEventSource(new lambdaEventSources.SqsEventSource(queue));

// CDK automatically grants the Lambda permission to read from the queue
```

CDK automatically wires [[AWS IAM|IAM]] permissions between constructs — one of its biggest time-saving features.

---

## CDK CLI Commands

| Command | Description |
|---|---|
| `cdk init` | Create a new CDK project |
| `cdk synth` | Generate CloudFormation template without deploying |
| `cdk diff` | Show what will change before deploying |
| `cdk deploy` | Deploy the stack to AWS |
| `cdk destroy` | Tear down all resources in the stack |

---

## CDK vs Terraform

| CDK | Terraform |
|---|---|
| AWS-specific | Multi-cloud |
| Uses programming languages | HCL (HashiCorp Configuration Language) |
| Generates CloudFormation | Own state management |
| Better AWS service coverage | Better for multi-cloud |

CDK for Terraform (cdktf) allows using CDK syntax with Terraform backends.

---

## Related Topics

- [[Cloud Native]] — CDK is an Infrastructure as Code tool, a core cloud-native practice
- [[AWS Lambda]] — CDK is the recommended way to define and deploy Lambda functions
- [[AWS SQS]] — CDK auto-wires IAM permissions when connecting Lambda to SQS
- [[AWS IAM]] — CDK generates and manages IAM roles and policies automatically
