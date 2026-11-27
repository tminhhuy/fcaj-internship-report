---
title: "Deploy the stack"
date: 2026-09-07
weight: 2
chapter: false
pre: " <b> 5.7.2. </b> "
---

#### 1. Check that the model can be called

```bash
aws bedrock-runtime converse --region ap-southeast-1 --model-id apac.amazon.nova-pro-v1:0 \
  --messages '[{"role":"user","content":[{"text":"Xin chao"}]}]' \
  --query "output.message.content[0].text" --output text
```

A short greeting in reply means the account can use Nova Pro through the APAC inference profile.

#### 2. Call Cohere Embed once with an administrator

Cohere Embed is a third-party model; the first call creates the AWS Marketplace subscription, which SCA's roles are deliberately not allowed to create.

```bash
aws bedrock-runtime invoke-model --region ap-southeast-1 --model-id cohere.embed-multilingual-v3 \
  --cli-binary-format raw-in-base64-out --body '{"texts":["xin chao"],"input_type":"search_document"}' /dev/null
```

#### 3. Build the web app and install the CDK app

```bash
git clone <repository> sca && cd sca
./scripts/build.sh
cd infra && python3 -m venv .venv && source .venv/bin/activate
pip install -r requirements-dev.txt
python -m pytest -q
```

#### 4. Bootstrap CDK

Bootstrap Singapore and us-east-1 (where the WAF for CloudFront lives).

```bash
npx aws-cdk@2.1149.0 bootstrap aws://<ACCOUNT_ID>/ap-southeast-1 aws://<ACCOUNT_ID>/us-east-1
```

#### 5. Configure and deploy

Edit the `sca` block in `infra/cdk.json` (`alertEmail`, `monthlyBudgetUsd`, `endpointAzs`; for a custom domain also `domainName` and `certificateArn`), then run:

```bash
npx aws-cdk@2.1149.0 deploy "Dev/*"
```

CDK prints `SiteUrl`, `CloudFrontDomain`, `UserPoolId` and `DashboardUrl`. For a custom domain, point the root record of the domain (ALIAS or CNAME) to `CloudFrontDomain`.

{{% notice tip %}}
If `cdk deploy` fails with `ModuleNotFoundError: aws_cdk`, CDK is running `python3 app.py` with another Python on the machine instead of the virtual environment. Pass the interpreter explicitly: `npx aws-cdk@2.1149.0 deploy "Dev/*" --app ".venv/bin/python3 app.py"`.
{{% /notice %}}

#### 6. Load the law library

A new stack starts with an empty Knowledge Base. Because vbpl.vn blocks requests from AWS, all 8 laws were built by hand from the official full text with `scripts/corpus.py build` and stored in `legal-corpus/laws/`. The next deployment uploads them to S3, `kb-sync` runs the ingestion job, and `kb-eval` emails the benchmark results. Until the library is loaded, every citation is removed.

#### 7. Create the first user

Open `SiteUrl` → **Sign up**, enter the code sent by email, then set up two-step verification with an authenticator app.

#### Using a custom domain (smartca.app)

1. Request a free public certificate in ACM in **us-east-1** (CloudFront only accepts certificates from that Region) for `smartca.app` (add `www.smartca.app` if you serve it too).
2. Add the CNAME validation records at the DNS provider and wait for the status `ISSUED`.
3. Set `domainName` and `certificateArn` for the stage in `infra/cdk.json` and deploy again.
4. Point the root record and `www` to the CloudFront domain. The `.app` top-level domain always requires HTTPS, which CloudFront provides.
