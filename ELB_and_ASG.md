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
### Creating ELB
> Navigate to `Load Balancer` under <ins>Load balancing</ins>
> 1. Select Loa
> 2. 


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
>     - Provide the necessary features like load balancing algorithms desired, health checks, sticky sessions, and SSL termination.
>  5. You re done test your resources by calling it via the ELB is entry point. 
 
   

 



