## IAM Authentication & Security:
- In AWS two main types of authentication include `conole(web-interface)` and `programatic(CLI, AWS PowerShell tools)`.</br>
`Console access` authenticates using a `password, and MFA if inabled`.</br>
`Programmatic access` authenticates using an `Access Key ID, and Secret Access Key`.</br>
**Authentication Best Practices**:
  - 

## Users
## Groups
## Policies
## Roles
- IAM roles are a secure way to grant permission to entities that you trust, IAM roles issue keys that are valid for short durations, making them a more secure way to grant access. </br>
- **Entities example**: 
  - IAM user in another account. 
  - Application code running on an EC2 instance needs to perfor actions on AWS resources. 
  - An AWS service that needs to act on resources in your account to provide its feature ex:EKS need role for EC2 to create its node groupe. 
  - Users from a corporate directory who use identity federation with SAML.
- **Types of Trusted Entity**: (AWS service, Another AWS account, Web dentity, SAML 2.0 federation)
- **Roles Attributes**: name, description, Trusted entities(Aws service, Web, ... ), Policies, Permissions boundary, Maximum session duration(default 1h), ARN, Instance Profile ARNs.
**common usecases**:
- Roles are commonly used to grant EC2 instances, permission to execute actions on S3 storage.
  - Ex: giving an EC2/Lambda/Others Instances permission to Read/write objects to an S3 bucket.
  - **Note**: an EC2/Lamdba/Others instances deployed by any IAM identity, does not have permission to execute any action on S3, there for a role is required for that.
  
-  
