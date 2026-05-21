# AWS Lambda

[AWS Lambda Documentation](https://docs.aws.amazon.com/lambda/latest/dg/) | [Lambda Best Practices](https://docs.aws.amazon.com/lambda/latest/dg/best-practices.html)

AWS Lambda is AWS's serverless compute service. You upload a function, configure a trigger, and AWS runs it on demand — managing all underlying infrastructure automatically. See [[Serverless]] for the general concept.

---

## Function Structure

A Lambda function has a **handler** — the entry point AWS calls on each invocation:

```kotlin
// Kotlin example with AWS Lambda Kotlin SDK
fun handler(event: APIGatewayProxyRequestEvent, context: Context): APIGatewayProxyResponseEvent {
    val body = event.body
    // process...
    return APIGatewayProxyResponseEvent()
        .withStatusCode(200)
        .withBody("""{"status": "ok"}""")
}
```

The `event` type varies by trigger source (API Gateway, SQS, S3, etc.).

---

## Triggers

| Trigger | Event source | Common use case |
|---|---|---|
| **HTTP** | API Gateway / Function URL | REST API endpoint |
| **Queue** | [[AWS SQS]] | Background job processing |
| **Topic** | [[AWS SNS]] | React to broadcast events |
| **Schedule** | EventBridge (cron) | Nightly jobs, cleanup tasks |
| **Storage** | S3 event | Process uploaded files |
| **Database** | DynamoDB Streams / RDS Proxy | React to data changes |
| **Another Lambda** | Direct invocation | Chained workflows |

---

## Execution Model

1. Trigger fires
2. AWS provisions a runtime environment (if no warm instance exists)
3. Handler is called with the event payload
4. Function executes (max 15 minutes)
5. Environment idles or is terminated

**Cold start:** First invocation after idle period incurs initialisation overhead (~100ms–1s). Mitigate with Provisioned Concurrency (keeps instances warm at cost).

**Stateless:** No in-memory state persists between invocations. External storage (DynamoDB, S3, ElastiCache) must be used for state.

---

## Limits

| Limit | Value |
|---|---|
| Max execution time | 15 minutes |
| Memory | 128 MB – 10 GB |
| Ephemeral storage (/tmp) | 512 MB – 10 GB |
| Deployment package | 50 MB (zipped), 250 MB (unzipped) |
| Concurrent executions | 1,000 (default, can be raised) |

---

## IAM and Permissions

Every Lambda function runs as an [[IAM]] Role (execution role). The role's policies determine what AWS services the function can access:

```json
{
  "Effect": "Allow",
  "Action": ["sqs:ReceiveMessage", "sqs:DeleteMessage"],
  "Resource": "arn:aws:sqs:eu-central-1:123456789:kwizz-jobs"
}
```

Lambda also has a **resource policy** controlling which services are allowed to invoke it.

---

## Lambda in the Serverless Stack

Classic pattern:
```
Internet
    ↓
API Gateway  (routing, auth, rate limiting)
    ↓
Lambda       (business logic)
    ↓
DynamoDB / RDS / S3  (data)
```

Lambda + [[AWS SQS]] for async processing:
```
API Gateway → Lambda (enqueue) → SQS → Lambda (process) → DynamoDB
```

---

## VPC Integration

By default Lambda runs outside any [[AWS VPC|VPC]] (can reach the internet but not private resources). To access RDS in a private subnet, configure Lambda to run inside a VPC subnet.

**Tradeoff:** VPC Lambda has higher cold start due to ENI provisioning. Use where needed; avoid by default.

---

## Related Topics

- [[Serverless]] — Lambda is AWS's implementation of the serverless compute model
- [[AWS IAM]] — every Lambda must have an execution role
- [[AWS SQS]] — primary trigger for async, event-driven Lambda processing
- [[AWS SNS]] — Lambda subscribes to SNS topics for broadcast event handling
- [[API Gateway]] — front door for HTTP-triggered Lambda functions
- [[AWS VPC]] — configure when Lambda needs access to private resources
