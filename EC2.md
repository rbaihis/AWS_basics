# AWS EC2
>EC2 is the computational backbone of AWS.</br>

>Virtualisation is the process of creating a software-based virtual representation of a Hardware-Resource (server, router, networks, storage ,etc).</br>

>When deploying an EC2 instance, we re creating a virtual representation of a physical server and it's resources(CPU,GPU,Network,Storage).</br>

>EC2 can have diffirent configurations.
>AWS offers a highly customizable set of configuration options for EC2 instances.</br>
>EC2 Configuration settings can be adapted to resource needs in real-time.

## Useful Links
- [EC2 instance types](https://aws.amazon.com/ec2/instance-types)
- [Storage for Root device type](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ComponentsAMIs.html#storage-for-the-root-device)
- [Finding an AMI that meet your EC2 requirement](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/finding-an-ami.html)
  
## Navigation
- [EC2 Instance](#EC2-Instance)
  - [EC2 Instance Core Feature](#EC2-Instance-Core-Feature)
  - [EC2 Instances Types - Flavors](#EC2-Instances-Types---Flavors)
- [AMI - Amazon Machine Image](#AMI---Amazon-Machine-Image)
  - [Creating AMI From Existing Instance](#Creating-AMI-From-Existing-Instance)
- [Elastic IP](#Elastic-IP)
- [Working With ElasticIPs](#Working-With-ElasticIPs)
- [Security Groups](#Security-Groups)
- [IAM Roles With EC2](#IAM-Roles-With-EC2)
  - [Working with IAM Roles and EC2](#Working-with-IAM-Roles-and-EC2)
- [Launch an EC2 Instance From Template](#Launch-an-EC2-Instance-From-Template)
  - [Generating A launch Template](#Generating-A-launch-Template)
- [SSH With EC2](#SSH-With-EC2)
  
## EC2 Instance 
>EC2 instances are scalable virtual computing environments.

>AWS offers pre-configured software packages that include an operating system and other applications that are pre installedon deployment. These templates are called `AMIs (Amazon Machine Images)`.

>There are variety of configurations for instances, known as `Instances Types`. these configuration includes `(CPU,Memory,Storage,Network-Capacity,GPU)`.

>Whatever operation can be done on a Physical server can be done with EC2 intances.

>To access your EC2 instance you can use SSH connection or AWS-Console.

>EC2 can have `Multiple types of Storage Volumes`, both temporary and persistant.
  - Temporary storage is known as instance storage volume, all data get deleted from storage when instance stopped, hibernated, or terminated.
  - `EBS (Elastic Bloc Store)` volumes offer persistant storage for data that is not to be deleted when an instance changes its operational state.
  - `Note` type of EC2 instances matters when chosing a persistant volume since some type offers multiple choices while others only offers ESB only ass attachment Volumes.

>EC2 instances, and ESB volumes can be launched in multiple regions and availibility zones.

>`Firewalls` can be configurated through AWS. using `Security Groups`.

>when deployed, each instance has an IPv4 address standard and Public , this Public feature is called `(Elastic IP)`, ELastic IPs can be associated and disassiociated from EC2 instance which makes them flexible and transfable under the same region.

>`Note that` if disassociated the ElasticIP, you still have publicIPs, but PublicIp that re not ElasticIPs can changes and are not flexibal and reliable as the ElasticIPs.
>  - EIPs provide a static IP address that remains constant, even if the underlying instance is stopped, started, or terminated.

### EC2 Instance Core Feature
- Deploy virtual machines
- Storing Data both persistent(EBS) and temporary drives(Instance Store Volume).
- Distributing network load across multiple EC2 replicas instances through `ELB (Elastic Load Balancing)`.
- Ability to scale services using `ASG (Auto-Scaling Groups).`

### EC2 Instances Types - Flavors
- EC2 instance can be configured with diffirent types of `virtual hardware`.
- Each instance type offers the ability to customize CPU, Memory, Storage, Network Resources such as bandwidth.
- Each resource(CPU,RAM,etc) can be scaled up Or Down at any time, depending on application requirements.
- AWS offers a substantial number of instances types and AMIs.
- `Note` Tye of EC2 instances matters when chosing a persistant volumes, network Interfaces, Pods hosted numbers, consider limitations on your choice.
- **Instance-Types Categories based on use cases**:
  - [useful Link about instance types](https://aws.amazon.com/ec2/instance-types)
  - General Purpose -> family : T2, T3, T3a, M5, M6i, M6gd, etc
  - Compute Optimized -> family : C5, C6i, C6gd, etc
  - Memory Optimized -> family : R5, R6i, R6gd, etc
  - Storage Optimized -> family : D2, D3, I3, I4i, etc
  - Accelarated Computing -> family : P4d, G5, G6, Trn1, Inf1, Inf2, DL1, DL2q, F1, etc
  - HPC Optimized -> family : Hpc
  - **Important Notes**
    - Some Types may offer extra charge since they have `Credit` for `over utilisation` of credit.
    - **Read More about these Types before Choosing One**
      - **Burstable Performance** under general purpose category like t2, t3

## AMI - Amazon Machine Image
- An AMi contains the initial configuration information required to deploy an EC2 instance (think of it as a dockerImage or VMx-Image for VMware).
- An AMI `must be specified` when an instance is lanched.
- An AMI image can be linux, windows, loadbalancer AMI, Router AMI, etc
- **An AMI Contains**:
  - **Launch Permissions**:
    - `Public` - The owner grants launch permissions to all AWS accounts.
    - `Explicit` - The owner grants launch permissions to specefic AWS accounts.
    - `Implicit` - The owner has implicit launch permissions for an AMI.
  - AMIs are categorized as either `backed by Amazon EBS`, or `backed by Instance Store`.
  - **An AMI deployed using Amazon EBS**, is done so using an EBS snapshot (S-Shot is a point-in-time copy of an amazon EBS volume).
  - **An AMI launched from an instance Store**, is created using a template stored in Amazon S3 (Like glance images stored in Swift in an openstack environment). 
  - In both cases the EBS or Instance store contains the data regarding the operating system, application server, and applications to be installed on instance deployment.
  - `Diffirence` between both two methods [here for more info](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ComponentsAMIs.html#storage-for-the-root-device)
  - **Block Mapping**:
    - An AMI also `contains a block device mapping` which specifies `the persistent storage volume to attach`. This means when a snapshot is taken, it stores information regarding each volume that is attached to.
  - **Configuration Options Can Be Specified**:
    - AWS Region
    - Operating System (Linux or Windows)
    - CPU, Memory, Storage Space (for minimum or averace resource requirement for smoothly running)
    - Network Card: Performance, Public IP
    - Firewall Rules: Security Groups
    - Architecture (32-bit or 64-bit)
    - Launch Permissions
    - Storage for the root device: (EBS or Instance Store-Backed AMI)
  - **AMI Categories**:
    - `AWS Marketplace AMIs` - AMIs which are developed by third parties and available in AWS (Cost associated for using it).
    - `Community AMIs` - AMIs developed by the community based projects can be used under a general license (Free).
    - `MyAMIs` - AMIs which are created by the account owner.
### Creating AMI From Existing Instance:
- this approach below is similar to `docker commit runningContainer newImageGeneratedName` but with VMs (Ignore it if unfamiliar with docker).
- when creating an AMI from an existing EC2 instance, it will contain the following informations:
  - All of the information included in a Launch Template.
  - Software related component such as OS, App-Server, and any applications currently exist in instance.
  - One or More (Amazon EBS) snapshots.
  - Launch permissions that control wich AWS accounts can use the AMI to launch instances.
  - A block device mapping that specifies the volumes to attach to the instance when its launched.
![Creating AMI Image from my Existing Instance](images/createAMIfromInstance.gif)

## Elastic IP
- A reserved IP which is assigned to your account, on creation.
- Can be associated and disassociated from EC2 instances.
- Dynamic - Transferable between EC2 instances within a particular region.
- You are `billed` for elastic IPs which are not associated with an instance by default.
- An Elastic Ip can be easily deleted once created.
- Before deleting an Elastic IP, it's good practice to confirm it is not associated with any operational Instance.
- Flexibility for 0 down time :
  - `Administrator duplicate instnace prod > update new instance > test new instance > disassoiate ElasticIP from instance prod > assosiate it to the new updated instance`
  - **Many other use cases for ElasticIP flexibilities can be benificial**.
    - Dynamic Scaling: To add or remove instances without affecting the public IP address of your service.
    - DNS Records: To maintain stable DNS records that point to your service.
    - Efficient Resource Utilization
    - Security Groups: You can associate security groups with EIPs to control inbound and outbound traffic, enhancing security.
    - EIPs provide a static IP address that remains constant, even if the underlying instance is stopped, started, or terminated.
### Working With ElasticIPs
- **Creating ElasticIp and Assign it to Instance**:
  1. `Network & Security > Elastic IPs > Allocate ElasticIP address > RegionMatchInstanceRegion > Allocate > Name it(optional)`
  2. `Actions > Associate ElasticIp > Resource Type (select instance) > Instance (chose the desired Instance) > Associate`

## Security Groups
- A security group acts as a virtual firexall for your instance to control inbound and outbound traffic.
- When an Instance is launched within a `VPC`, it can be assigned to up to `5 Security Groups`.
- If an EC2 instance is launched `without specifing a Security Group`, it will automatically be assigned to the `Default Security Group` for the `VPC`.
- A Security Group is composed of `rules`. these rules `define the parameters of the firewall which guards your instances`.
- The rules are enforced by filtering traffic based on `protocols and port numbers`.
- Rules for Security group can be `added or removed`. (also refered to as authorizing or revoking inbound or outbound access).
- The security group rules are divides into inbound and outbound rules.
- You can define separate rules for both inbound and outbound traffic.
- AWS permits specifying `Allow rules`, but `Not deny rules`.
- When a Security group is created, it has no inbound rules by default. This means all inbound traffic originating from another host will be denied until inbound rules are configured (Least Privelege Concept).
- By Default, a security group will have a rule that allows all outbound traffic, can be configured to restrict outbound traffic.
- Default Security Group can not be deleted.
- **Rules contains**:
  - `Type`(custom,All,Single) : All traffic, HTTP, SSH, POP3 ICMP(ping), IMAPS, SMTP, NFS, etc
    - (All, Custom) are for Common layer transfer protocols such UDP, TCP, ICMP.
    - (Single) are for Popular types such  HTTP, SSH, POP3 ICMP(ping), IMAPS, SMTP, NFS, DNS(UDP), DNS(TCP), MSSQL, SMB, LDAP, etc
  - `Protocol`: TCP, UDP
  - `Port range`: can be a range or a single value
  - `Source`(Inbound Rule): From Where expecting traffic, 0.0.0.0/0-IPv4 or ::/0-IPv6 -> does not matter from where it comming.
  - `Destination`(Outbound Rule): To Where traffic allowed to go , 0.0.0.0/0-IPv4 or ::/0-IPv6 -> does not matter To where.
  - `Description`: (optional for readability)
### Working With Security Group
- Create a Security Group
  - Name it
  - Specify the VPC where ur expected instance(s) will be attached to.
  - Check the Default Rules if they re convinient to your use case.
  - Modify, remove or add Rules as you see fit.
  - Create Security Group
  - Finally add The Security to Your Instance.

## IAM Roles With EC2
- IAM roles can be used within an AWS resource, to grant permissions to applications running on the EC2 instance in our case, to access specified services and resources on AWS.
### Working with IAM Roles and EC2
- In this example we will `create a Role full access to S3 service` and assign it to the EC2 instance
1. Navigate to IAM-Roles console
2. Create Role name it ec2s3fullaccess
3. Chose type of `trusted entities` as **service**.
4. Attach the policy `AmazonS3FullAccess` to the role.
5. Update session(optionally DefaultVal:1h)
6. Create the Role.
7. Navigate back to the EC2 instance
8. Right click on the instance
9. Select Security > Modify IAM role > Select the `ec2s3fullaccess` > save
10. A success message will appear => Now that the IAM-Role is attached to the instance, any application running on the instance `will have full access to perform any actions on the S3 service`

## Launch an EC2 Instance From Template
- A `Launch Template` offers a more efficient way to launch one or multiple instances based on saved configuration setting from previous instances.
- Avoid having to specify:
  - Instance Type, Network Settings, Storage, Security Groups, Keys, Roles, etc.
- Instance templates can be versioned.
- Each version of an instance template can have unique launch parameters if needed.
- **Ways to Create Launch Template**:
  - From scratch
  - Generating it from an existance instance that you wish to have the template from.
  - From an other Template that u desire to do new version from It.
### Generating A launch Template
- in this example lets assume we will generate it from an existing template.
- **Creation Steps**:
  1. Navigate to the desired EC2 instance
  2. Right click > Image and templates > Create Template From Instance
  3. Name it > Description(useful) > Tag it for best practices
  4. Ignore EC2 auto scaling for now
  5. Launch template content > Check the Security Group desired > Other option (keep it Or modify it, based on your use case)
  6. Before you save it > Advanced Details > User Data (check if you want what is there or not)
  7. Create Launch template
- **Steps to Launch Instance based on Launch Template**
  1. Navigate to EC2 Instance Launch Template
  2. Select the desired Template > Actions > Launch Instance from Template
  3. Done its creating.

## SSH With EC2
- SSH is a secure protocol that allows you to access your server remotely and perform many action on it.
- Prerequirement:
  1. Must have created an SSH-KEY and added the public key during instance creation to the EC2.
  2. Store the SSH-keys in your SSH-client in ~/.ssh directory.
  3. Your Security Group attached to the EC2 must allow Inbound TCP traffic to Port 22 typically Default unless changed.
  4. Make sure to add Rule SSH with Protocol TCP on Port 22 From Source 0.0.0.0/0 if ur remote SSH client uses Dynamic IP-Addresses.
  5. finally SSH to your Remote Server EC2 instance:
     - `ssh ec2Username@instanceDnsOrIpAddress -i ~/.ssh/yourPrivateKey` => and you re connected.
