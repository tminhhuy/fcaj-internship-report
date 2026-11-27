---
title: "Triển khai stack"
date: 2026-09-07
weight: 2
chapter: false
pre: " <b> 5.7.2. </b> "
---

#### 1. Kiểm tra quyền gọi model

```bash
aws bedrock-runtime converse --region ap-southeast-1 --model-id apac.amazon.nova-pro-v1:0 \
  --messages '[{"role":"user","content":[{"text":"Xin chao"}]}]' \
  --query "output.message.content[0].text" --output text
```

Nhận được một câu chào ngắn nghĩa là tài khoản dùng được Nova Pro qua inference profile APAC.

#### 2. Gọi Cohere Embed một lần bằng tài khoản quản trị

Cohere Embed là model bên thứ ba; lần gọi đầu tạo đăng ký AWS Marketplace mà role của SCA cố ý không có quyền tạo.

```bash
aws bedrock-runtime invoke-model --region ap-southeast-1 --model-id cohere.embed-multilingual-v3 \
  --cli-binary-format raw-in-base64-out --body '{"texts":["xin chao"],"input_type":"search_document"}' /dev/null
```

#### 3. Build web app và cài ứng dụng CDK

```bash
git clone <repository> sca && cd sca
./scripts/build.sh
cd infra && python3 -m venv .venv && source .venv/bin/activate
pip install -r requirements-dev.txt
python -m pytest -q
```

#### 4. Bootstrap CDK

Bootstrap cho Singapore và us-east-1 (nơi chứa WAF của CloudFront).

```bash
npx aws-cdk@2.1149.0 bootstrap aws://<ACCOUNT_ID>/ap-southeast-1 aws://<ACCOUNT_ID>/us-east-1
```

#### 5. Cấu hình và triển khai

Sửa khối `sca` trong `infra/cdk.json` (`alertEmail`, `monthlyBudgetUsd`, `endpointAzs`; với domain riêng thêm `domainName` và `certificateArn`), rồi chạy:

```bash
npx aws-cdk@2.1149.0 deploy "Dev/*"
```

CDK in ra `SiteUrl`, `CloudFrontDomain`, `UserPoolId`, `DashboardUrl`. Với domain riêng, trỏ bản ghi gốc của domain (ALIAS hoặc CNAME) về `CloudFrontDomain`.

{{% notice tip %}}
Nếu `cdk deploy` báo `ModuleNotFoundError: aws_cdk`, CDK đang gọi `python3 app.py` bằng bản Python khác của máy thay vì môi trường ảo. Chỉ rõ trình thông dịch: `npx aws-cdk@2.1149.0 deploy "Dev/*" --app ".venv/bin/python3 app.py"`.
{{% /notice %}}

#### 6. Nạp kho luật

Stack mới có Knowledge Base rỗng. Vì vbpl.vn chặn yêu cầu từ AWS, cả 8 luật được dựng tay từ toàn văn chính thức bằng `scripts/corpus.py build` và nằm trong `legal-corpus/laws/`; lần deploy kế tiếp đưa chúng lên S3, `kb-sync` chạy ingestion và `kb-eval` gửi email kết quả câu hỏi chuẩn. Chưa nạp thì mọi trích dẫn bị loại.

#### 7. Tạo người dùng đầu tiên

Mở `SiteUrl` → **Sign up**, nhập mã gửi qua email, rồi cài xác thực hai bước bằng ứng dụng authenticator.

#### Gắn domain riêng (smartca.app)

1. Xin chứng chỉ công khai miễn phí trong ACM ở **us-east-1** (CloudFront chỉ nhận chứng chỉ ở Region này) cho `smartca.app` (thêm `www.smartca.app` nếu dùng cả tên này).
2. Thêm bản ghi CNAME xác minh ở nhà cung cấp DNS và chờ trạng thái `ISSUED`.
3. Khai `domainName` và `certificateArn` cho stage trong `infra/cdk.json` rồi deploy lại.
4. Trỏ bản ghi gốc và `www` về domain CloudFront. Đuôi `.app` luôn bắt buộc HTTPS, CloudFront đáp ứng việc này.
