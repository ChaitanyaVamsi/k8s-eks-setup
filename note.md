# 🚀 IAM Permissions Required for eksctl

To create an EKS cluster using `eksctl`, your AWS identity (IAM user or role) must have sufficient permissions.

other wise you get this error

eksctl create cluster --config-file=eks.yaml
Error: checking AWS STS access – cannot get role ARN for current session: operation error STS: GetCallerIdentity, get identity: get credentials: failed to refresh cached credentials, no EC2 IMDS role found, operation error ec2imds: GetMetadata, http response error StatusCode: 404, request to EC2 IMDS failed


---

## 🔹 Option 1: Quick Setup (For Testing Only)

Attach the following AWS managed policy:

- `AdministratorAccess`

> ⚠️ This provides full access. Use only for testing or learning.

---

## 🔹 Option 2: Minimum Required Permissions (Production)

Create an IAM role/user and attach these AWS managed policies:

### ✅ Required Policies

- `AmazonEKSClusterPolicy`
- `AmazonEKSServicePolicy`
- `AmazonEKSWorkerNodePolicy`
- `AmazonEC2FullAccess`
- `AmazonVPCFullAccess`
- `IAMFullAccess`
- `CloudFormationFullAccess`

---

## 🔹 Why These Permissions Are Needed

`eksctl` provisions infrastructure using multiple AWS services:

| Service            | Purpose                      |
|-------------------|------------------------------|
| Amazon EKS        | Cluster creation             |
| Amazon EC2        | Worker nodes                 |
| CloudFormation    | Infrastructure provisioning  |
| AWS IAM           | Roles and permissions        |
| Amazon VPC        | Networking setup             |

---

## 🔹 Option 3: Custom Least-Privilege Policy (Advanced)

For tighter security, define a custom IAM policy with scoped permissions:

### ✅ Required Actions

**EKS**
- `eks:*`

**EC2**
- `ec2:*`

**IAM**
- `iam:CreateRole`
- `iam:AttachRolePolicy`
- `iam:PassRole`

**CloudFormation**
- `cloudformation:*`

**VPC (EC2 Networking)**
- `ec2:CreateVpc`
- `ec2:CreateSubnet`
- `ec2:CreateSecurityGroup`

---

## 🔹 Attach IAM Role to EC2 (If Running on Instance)

1. Go to **AWS Console → EC2**
2. Select your instance
3. Click: **Actions → Security → Modify IAM Role**
4. Attach the IAM role created above

---

## 🔹 Verify Access

Run the following command:

```bash
aws sts get-caller-identity
```

## if not getting response froms eks
```bash
aws eks update-kubeconfig --region us-east-1 --name sampleapp
```
