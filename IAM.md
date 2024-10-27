## IAM Authentication & Security
- In AWS two main types of authentication include `conole(web-interface)` and `programatic(CLI, AWS PowerShell tools)`.
- `Console access` authenticates using a `password, and MFA if inabled`.
- `Programmatic access` authenticates using an `Access Key ID, and Secret Access Key`.
- **Authentication Best Practices**:
  - Each IAM user has `their own account`, no sharing accounts between multiple users for better audit by user.
  - Never sharing or publicly posting either a pw or secret access hey.
  - store access keys and secret access keys securely. preferably in an encrypted file.
  - Disable access keys that have not been used for some time.
  - Delete IAM users that are no longer needed on the AWS account.
  - Enabling MFA on the main root account, and any IAM user accounts.
  - When Possible , Use `IAM ROLES` to access AWS resources, rather than programmatically. -> this way the access key ID does not have to be hardcodes into the application.
    - **Benefits**: when configured correctly IAM will dynamically manage the credentials for you with temporary credentials hat are rotated automatically.
    - **Note**: `only possible` if recources requiring access are `running inside AWS`.
  - **If Service Outside AWS**: require `programmatic access`, it's best to `create dedicated service accounts and policies` specifically for `each use case`.
    - (programmatic permission for specefic action)[/images/customPolicy.jpg]
  - 
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
