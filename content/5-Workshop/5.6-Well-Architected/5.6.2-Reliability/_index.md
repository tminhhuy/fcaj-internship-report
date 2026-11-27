---
title: "Reliability"
date: 2026-09-07
weight: 2
chapter: false
pre: " <b> 5.6.2. </b> "
---

The weakest point is the Amazon Bedrock token quota, so the system pushes load into a queue instead of dropping jobs, and every failure leaves a clear status and an alert.

| Failure | What the system does | Recovery |
| --- | --- | --- |
| Many contracts uploaded at once | SQS accepts every job; `starter` only starts an execution when fewer than 5 are running; at 5, the job becomes `QUEUED` and the message is sent again after 60 seconds | Automatic, no job is dropped |
| Bedrock or Textract throttling, transient errors | Retry after 2 seconds, doubling each time, up to 6 attempts, full jitter | Automatic; an alarm if throttling persists |
| Error that does not go away | MarkFailed writes `FAILED` and the error code to DynamoDB; ExecutionsFailed alarm | Fix the cause, then the user uploads again |
| A message cannot start a workflow | After 3 receives it goes to the DLQ and is kept for 14 days; uploads-dlq alarm | Redrive to the main queue |
| Duplicate S3 events, the same file uploaded again | Idempotency key (SHA-256 of the file + company code) written with a condition; the execution is named after the job, so it cannot run twice | The new job becomes `DUPLICATE` and points to the earlier report |
| An agent runs too long | Agent Lambda timeout 240 seconds; execution limit 15 minutes; ExecutionsTimedOut alarm | Retry or MarkFailed |
| The model returns JSON that breaks the schema | Sent back for correction at most 2 times | Then MarkFailed |
| One Availability Zone fails | Lambda functions and interface endpoints span 2 AZs; S3, DynamoDB, SQS and Step Functions are multi-AZ regional services | Automatic (the dev stage puts endpoints in 1 AZ to save cost) |
| Data lost or overwritten | DynamoDB point-in-time recovery; the original in `vault` is protected by Object Lock | Restore the table to a point in time; the original cannot be changed |
| The whole environment is lost | All infrastructure is code | Rebuild with `cdk deploy`, about 30 minutes |

#### Why no reserved concurrency

Reserved concurrency is not used to limit load: `StartExecution` returns immediately, so limiting the number of Lambda instances does not limit the number of executions, and a new account only has a Lambda concurrency quota of 10. Waiting for a free slot is not counted as a failure: a message that is sent back starts again from its first receive.

#### Still to do

- Run the load test with 50 contracts in one minute and confirm that the queue drains with 0 messages in the DLQ.
