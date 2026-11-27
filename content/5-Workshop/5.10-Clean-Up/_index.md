---
title: "Clean up"
date: 2026-09-07
weight: 10
chapter: false
pre: " <b> 5.10. </b> "
---

Remove every resource after the demo so that nothing keeps charging, especially the VPC interface endpoints, AWS WAF and the KMS keys, which are fixed monthly costs ([5.6.5 Cost Optimization]({{< relref "/5-Workshop/5.6-Well-Architected/5.6.5-Cost-Optimization" >}})).

#### 1. Delete the stacks and the locked vault bucket

```bash
export AWS_REGION=ap-southeast-1
export ACCOUNT_ID=$(aws sts get-caller-identity --query Account --output text)

# 1. Delete every CDK stack (buckets use autoDeleteObjects)
cd infra && npx aws-cdk@2.1149.0 destroy --all --force

# 2. The vault bucket uses Object Lock (governance, 1 day in workshop): delete versions with bypass, then the bucket
aws s3api list-object-versions --bucket sca-vault-$ACCOUNT_ID \
  --query '{Objects: Versions[].{Key: Key, VersionId: VersionId}}' --output json > versions.json
aws s3api delete-objects --bucket sca-vault-$ACCOUNT_ID \
  --delete file://versions.json --bypass-governance-retention
aws s3api delete-bucket --bucket sca-vault-$ACCOUNT_ID

# 3. Check nothing tagged with the project is left
aws resourcegroupstaggingapi get-resources --tag-filters Key=project,Values=sca
```

#### 2. After the stacks are gone

| Item | What to do |
| --- | --- |
| KMS keys | They keep charging until they are fully deleted (minimum waiting period 7 days); confirm they are scheduled for deletion |
| ACM certificate and DNS records for smartca.app | No AWS charge, but remove the records that point to CloudFront after the stacks are deleted |
| CDK bootstrap stacks | Keep them if you will deploy again; otherwise delete `CDKToolkit` in ap-southeast-1 and us-east-1 and empty its bucket |
| Cost Explorer | Check again the next day that no cost with the tag `project=sca` remains |

The full procedure is in `docs/CLEANUP.md` in the project repository.
