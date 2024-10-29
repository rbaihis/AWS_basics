# ELB && ASG

**Refresher**</br>
Scalable AWS resources can be provisioned based on application requirements.</br>
We can scale vertically or horizontally Resources.</br>
In this part we will focus on `horizontally resource scaling` also known as `scaling In or Out`.</br>
`High availibility` infers the ability to store data redundantly across multiple Azs, as well as having your solution always up and functional and it's down-time close to 0 even in disasters.<br>
`Elasticity` is the ability to accuire resources as you need them and release thes resources when you no longer need them without the need for explicit human intervention.</br>
AWS high abilibility for cloud workloads can be achieved across three service groups (EC2, RDS and others MDB, Storage services[S3, EFS, EBS] ).</br>
>**For Max Availibiliy** from EC2, an architecture involving `ELB`, `Multiple AZs`, `ASGs` should be implemented.

>**`ELastic Load Balancers(ELB)` and `Auto-Scaling Groups(ASG)`**, Automatically and dynamically distribute traffic load between multiple instances.
>**Elasticity** can be `effectively managed` using two methods.
>>`Elastic Load Balancers (ELB)`
>>`Auto-Scaling Groups`

## Navigation
- [Load Balancer Overview](#Load-Balancer-Overview)
- [Elastic Load Balancer - ELB](#Elastic-Load-Balancer---ELB)
  - [ELB - Load Balancer Types](#ELB---Load-Balancer-Types)
- [](#)
- [](#)
- [](#)
- [ELB and ASG Together](#ELB-and-ASG-Together)
- [](#)
- [](#)
- [](#)
- [](#)
- [](#)
- [](#)
- [](#)
- [](#)

## Load Balancer Overview
Load balancer processes incoming requests based on preconfigured rules and distributes them amon several instances to ensure that all traffic is routed to healthy targets.
>If a single instance has maxed out its resource capacity, any additional network load will be routed to the remaing healthy instances within the group.
>The load balancer acts as a single point of access to an application and routinely `conducts health checks on all **registred targets**`.


## Elastic Load Balancer - ELB
> An `ELB` can be configured to direct incomming traffic by specifying one or more listeners.

> A `Listener` is a process that checks for connection requests, it is configured with `a protocol` and `port number` for connections from `Clients to the Load balancer`. Likewise, it is configure with a protocol and a port number for connections from `Load Balancer to the targets`.

> `Target Group` is **a group** of registred targets that are configured to handle `specified network traffic (typically Replicas)`.
 
### ELB - Load Balancer Types
- **Application Load Balancers**: (OSI Model `Layer-7 LB`) it `operates at the request level`.
  - ALBs are suited for HTTP/HTTPS requests.
  - ALBs offers a broad range of routing rules for incoming requests. this includes:
    - host-name, HTTP-Headers, SourceIP, PortNumber.
- **Network Load Balancers**: (OSI Model `Layer-4 LB`) `it operates at the connection level`.
  - NLBs are suited for TCP, UDP and TCP-TLS connections encrypted.
  - NLBs are `designed for very high performance workloads`.
- **GateWay Load Balnceres**: (OSI Model `Layer-3 LB`) `it operates at the Network level`.
  - They simply route traffic based on IP addresses and port numbers.
  - They are primarily used to distribute network traffic to network appliances like firewalls, intrusion detection systems, and VPNs.
  - `Note` do not assume that `Spring Gateway as GLBs` springGW uses internally a layer-7 ALBs to route traffic. 
- **Classic Load Balancers**: (to be dropped or already dropped from AWS)
### ELB Scheme
- `Internet-facing`: if you re expecting request coming from the internet and not from a private network having routing rules to your ELB.
- `internal`: an internal load balancer routes requests from clients to targets using private ip addresses.
### VPC Choice
- VPC can't be changed after creating ELB.
- its important that you chose a VPC that your target groups is also under it.
### Assign or Create a security Group
- Allow the necessary inbound rules usually is enough. usual cases (HTTP/HTTPS)
### Target Groups Types ELB Can Forward Traffic To
- `Check documentation for info`
- Instances
- Ip addressses
- Lambda Function (Only for ALB-ELBS)
- Application Load Balancer
### Creating ELB
> Navigate to `Load Balancer` under <ins>Load balancing</ins>
> 1. Select load balancer type (ALD - for HTTP/HTTPS) -> create
> 2. Name it
> 3. Specify Schema


## Auto-Scaling Groups - ASG
>An ASG contains a `collection of EC2s instances` that are `treated as a logical grouping` for the purpose of **automatic scaling and management**.

>ASG will **define** the `minimum and maximum number` of `Healthy` instances that **must** be in operations, to dynamically meet changing application load conditions.
>> this means the ASG can both Scale-Out until reaching maximum val, and Scale-In until it reachs its minimum val when load is low and vcan be handled by the minimum instances defined.

>ASG `define maximum` number of healthy instnaces, for billing purposes, mitigating unlimited scaling in case of DDOS attack, and for the CLoudProvider itself to have better predection for managing its own resources.

> ASG is considered a `single target` for the ELB, ELB relies on ASG is server registry to identify the inner targets to destribute traffic to them dynamically. So no manual adding or removing instances from the ELB when Scaling out or in.
> **summary**
- ASGs are designed to dynamically meet the changing needs of application load.
- ASGs are `perfect` for `keeping up with cyclical changes in application load` (scales In & Out).
- ASGs can also scale instance based on a `fixed schedule` (prediction case).
- ASGs can be scaled based on `custom scaling criteria using metrics from CloudWatch` (Sorry Prometheus can't do that without hooks).
- ASGs will auto deploy instances based on `pre-configured Launch templates`.
- ASGs `routinely` perform health checks on EC2 instances, and `replaces instances that fail the check`.
- An ASG can be configured to ensure a specified number of healthy instances which are operational at all times.
- ASGs can automatically register new instances to a load balancer.


## ELB + ASG Together
How to create an ELB integrated with an ASG.
>Steps:
>  1. Create a `Launch Template` for the EC2 instance to be grouped under the `ASG`.
>     - Defines the instance type, AMI, security groups, key pairs, EFS-config, EBS-config, IAM-Roles, and user data (for customization).
>     - Use a Pre-Configured tested custom AMI with all your (customization installed) for faster deployment and avoid the overhead of using `user data (for customization)`.  
>  3. Create your ASG and specify the desired `Lunch template`.
>     - Configure the min-max, and the other fields to answer to your expected load.
>     - Defines scaling policies based on metrics like CPU utilization, network traffic, or custom metrics.
>  4. Create your ELB of type ALBs, and add the ASG as a target
>     - Provide the necessary features like:
>       - scheme(internet-facing, internal), IP-Type(v4 or DualStack)
>       - Network mapping : VPC, Mappings(select at least 2 availibility zones & 1-subnet per zone).
>         - `Select as many zones as possible for the purpose of fault tolerance & high availibility`
>       - Security Group
>       - Listeners & routing:
>         - protocol, port
>         - `listener-target(Create a target groupe)`
>           - instances type since we want to point to ASG directly wich is of type instances
>           - Select the VPC
>           - Chose the Protocol Version based on your use case
>           - add path to the instance is `Health Check endpoint` for supervising its health. ex `/actuator/health`
>       - load balancing algorithms desired, health checks, sticky sessions, and SSL termination, tags.
>  5. `Default attributes will be created` **Updatable** after creation.
>  6. `Select the available instances(in our case the ASG group).
>     - Specify ports for the selected instances
>     - Click on `Include as pending below`
>  7. Create target group
>  8. Back to the ELB - Configuration page
>  9. `Attach` the Target Group
>  10. ADD-on services (optional- added charges) ignore it
>  11. Check the summuary before creating for any errors
>  12. Create a load Blancer > **Done**
>  13. Get your ELBès ip or DNS
>  14. Test your service by sending request to the load balancer.

 
   

 



