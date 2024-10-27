## Users
## Groups
## Policies
## Roles
- IAM roles are a secure way to grant permission to entities that you trust, IAM roles issue keys that are valid for short durations, making them a more secure way to grant access. </br>
**Entities example**: 
- IAM user in another account. 
- Application code running on an EC2 instance needs to perfor actions on AWS resources. 
- An AWS service that needs to act on resources in your account to provide its feature ex:EKS need role for EC2 to create its node groupe. 
- Users from a corporate directory who use identity federation with SAML.
</br>
- **Types of Trusted Entity**:
  -
- 
</br>
</br>
**common usecases**:
- Roles are commonly used to grant EC2 instances, permission to execute actions on S3 storage.
  - Ex: giving an EC Instances permission to Read/write objects to an S3 bucket.
  - Note: an EC2 instance deployed by any IAM identity, does not have permission to execute any action on S3, there for a role is required for that.
-   
-  
