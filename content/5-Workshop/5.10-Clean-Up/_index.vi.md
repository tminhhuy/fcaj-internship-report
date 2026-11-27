---
title: "Dọn dẹp tài nguyên"
date: 2026-09-07
weight: 10
chapter: false
pre: " <b> 5.10. </b> "
---

Gỡ toàn bộ tài nguyên sau demo để không còn gì tiếp tục tính phí, nhất là VPC interface endpoint, AWS WAF và khóa KMS vốn là phí cố định hằng tháng ([5.6.5 Tối ưu chi phí]({{< relref "/5-Workshop/5.6-Well-Architected/5.6.5-Cost-Optimization" >}})).

#### 1. Xóa các stack và bucket vault bị khóa

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

#### 2. Sau khi các stack đã xóa

| Hạng mục | Việc cần làm |
| --- | --- |
| Khóa KMS | Vẫn tính phí cho đến khi xóa hẳn (tối thiểu 7 ngày chờ); kiểm tra khóa đã ở trạng thái chờ xóa |
| Chứng chỉ ACM và bản ghi DNS của smartca.app | Không tốn phí AWS, nhưng nên xóa bản ghi trỏ về CloudFront sau khi gỡ stack |
| Stack bootstrap của CDK | Giữ lại nếu còn triển khai; nếu không, xóa `CDKToolkit` ở ap-southeast-1 và us-east-1 và làm rỗng bucket của nó |
| Cost Explorer | Kiểm tra lại vào ngày hôm sau, không còn chi phí nào gắn tag `project=sca` |

Quy trình đầy đủ trong `docs/CLEANUP.md` của repository dự án.
