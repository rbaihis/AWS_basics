# ELB && ASG

**Refresher**</br>
Scalable AWS resources can be provisioned based on application requirements.</br>
We can scale vertically or horizontally Resources.</br>
In this part we will focus on `horizontally resource scaling` also known as `scaling In or Out`.</br>
`High availibility` infers the ability to store data redundantly across multiple Azs, as well as having your solution always up and functional and it's down-time close to 0 even in disasters.<br>
`Elasticity` is the ability to accuire resources as you need them and release thes resources when you no longer need them without the need for explicit human intervention.</br>
AWS high abilibility for cloud workloads can be achieved across three service groups (EC2, RDS and others MDB, Storage services[S3, EFS, EBS] ).</br>
>**For Max Availibiliy** from EC2, an architecture invilving `ELB`, `Multiple AZs`, `ASGs` should be implemented.
>**`ELastic Load Balancers(ELB)` and `Auto-Scaling Groups(ASG)`**, Automatically and dynamically distribute traffic load between multiple instances.
>**Elasticity** can be `effectively managed` using two methods.
>>`Elastic Load Balancers (ELB)`
>>`Auto-Scaling Groups`

## Navigation
- [Elastic Load Balancer - ELB](#Elastic-Load-Balancer---ELB)
- [](#)
- [](#)
- [](#)
- [](#)
- [](#)
- [](#)
- [](#)
- [](#)
- [](#)
- [](#)
- [](#)
- [](#)
- [](#)

# Load Balancer Overview
Load balancer processes incoming requests based on preconfigured rules and distributes them amon several instances to ensure that all traffic is routed to healthy targets.
>If a single instance has maxed out its resource capacity, any additional network load will be routed to the remaing healthy instances within the group.
>The load balancer acts as a single point of access to an application and routinely `conducts health checks on all **registred targets**`.

## Elastic Load Balancer - ELB
> An `ELB` can be configured to direct incomming traffic by specifying one or more listeners.

> A `Listener` is a process that checks for connection requests, it is configured with `a protocol` and `port number` for connections from `Clients to the Load balancer`. Likewise, it is configure with a protocol and a port number for connections from `Load Balancer to the targets`.
> `Target Group` is **a group** of registred targets that are configured to handle `specified network traffic (typically Replicas)`.
### ELB Load Balancer Types:
- **Application Load Balancers**: (OSI Model `Layer-7 LB`) it `operates at the request level`.
  - ALBs are suited for HTTP/HTTPS requests.
  - ALBs offers a broad range of routing rules for incoming requests. this includes:
    - host-name, HTTP-Headers, SourceIP, PortNumber.
- **Network Load Balancers**: (ODI Model `Layer-4 LB`) `it operates at the connection level`.
  - NLBs are suited for TCP, UDP and TCP-TLS connections encrypted.
  - NLBs are `designed for very high performance workloads`.
- **GateWay Load Balnceres**:
- **Classic Load Balancers**: (to be dropped or already dropped from AWS)
