# AWS EC2
EC2 is the computational backbone of AWS.</br>
Virtualisation is the process of creating a software-based virtual representation of a Hardware-Resource (server, router, networks, storade ,etc).</br>
When deploying an EC2 instance, we re creating a virtual representation of a physical server and it's resources(CPU,GPU,Network,Storage).</br>
EC2 can have diffirent configurations.
AWS offers a highly customizable set of configuration options for EC2 instances.</br>
EC2 Configuration settings can be adapted to resource needs in real-time.

## Useful Links
- [EC2 instance types](https://aws.amazon.com/ec2/instance-types)
- [Storage for Root device type](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ComponentsAMIs.html#storage-for-the-root-device)
- [Finding an AMI that meet your EC2 requirement](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/finding-an-ami.html)
## Navigation
- [EC2 Instance](#EC2-Instance)
  - [EC2 Instance Core Feature](#EC2-Instance-Core-Feature)
  - [EC2 Instances Types - Flavors](#EC2-Instances-Types---Flavors)
- [AMI - Amazon Machine Image](AMI---Amazon-Machine-Image)
  - [Creating AMI From Existing Instance](#Creating-AMI-From-Existing-Instance)
- [Elastic IP](#Elastic-IP)
- [Working With ElasticIPs](#Working-With-ElasticIPs)
- []()
- []()
- #EC2 Instance
  - #EC2 Instance Core Feature
  - #EC2 Instances Types - Flavors
- #AMI - Amazon Machine Image
  - #Creating AMI From Existing Instance
- #Elastic IP
  - #Working With ElasticIPs

  
## EC2 Instance 
- EC2 instances are scalable virtual computing environments.
- AWS offers pre-configured software packages that include an operating system and other applications that are pre installedon deployment. These templates are called `AMIs (Amazon Machine Images)`.
- There are variety of configurations for instances, known as `Instances Types`. these configuration includes `(CPU,Memory,Storage,Network-Capacity,GPU)`.
- Whatever operation can be done on a Physical server can be done with EC2 intances.
- To access your EC2 instance you can use SSH connection or AWS-Console.
- EC2 can have `Multiple types of Storage Volumes`, both temporary and persistant.
  - Temporary storage is known as instance storage volume, all data get deleted from storage when instance stopped, hibernated, or terminated.
  - `EBS (Elastic Bloc Store)` volumes offer persistant storage for data that is not to be deleted when an instance changes its operational state.
  - `Note` type of EC2 instances matters when chosing a persistant volume since some type offers multiple choices while others only offers ESB only ass attachment Volumes.
- EC2 instances, and ESB volumes can be launched in multiple regions and availibility zones.
- `Firewalls` can be configurated through AWS. using `Security Groups`.
- Note That, when deployed, each instance has an IPv4 address standard and Public , this Public feature is called `(Elastic IP)`, ELastic IPs can be associated and disassiociated from EC2 instance which makes them flexible and transfable under the same region.

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
  - Many other use cases for ElasticIP flexibilities can be benificial.
### Working With ElasticIPs
