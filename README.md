# `DVA-C02`

# `AWS Well-Architected Framework`

- Operational excellence
- Security
- Reliability	
- Performance efficiency
- Cost optimization
- Sustainability

## The Shared Responsibility Model
**CUSTOMER** (Responsible for the security *in* the cloud)

- Customer Data
- Platform, Apps, IAM
- OS, Network & Firewall Configuration
- Client-side data encryption & data integrity authentication
- Server-side encryption
- Network Traffic Protection

**AWS** (Responsible for the security *of* the cloud)

- Software
- Compute, Storage, Database, Networking
- Hardware
- Regions, AZs, Edge locations

# `Identity & Access Management`

- Offers a centralized hub of control within AWS and integrates with all other AWS Services 
- Allows you to manage users and their level of access to the AWS console.

>When you create a new account the *root user* will be created which will have all permissions and access to all the resources.
**It's very important to secure your root user**

- `User` : Individual AWS user.
    - 5000 IAM users limit per account.
    - Any IAM user can only be member of 10 groups.

    >IAM user will be implicitely denied to usage and costs section even if he's admin  

- `Groups` : Collection of similar people with shared permissions & users will inherit permissions from the group.

    **It's best to inherit permissions from groups so that you don't have to manage them individually**

- `Roles` : Temporary Security Credentials. 
Assigned to any software service that needs to be granted permissions to do its job. 
    - Temporary Credentials are by generated by Secure Token Service (STS).

    It's similar to IAM user, however instead of being assiciated with one person , a role is intended to be assumed by anyone who needs it. 
    - Use roles in situations like cross account resource acceess.

    *Two types of policies are attached to a role :*
    - Trust Policy : Who can assume this role?
    - Permission Policy : What can they do after assuming the role?

- `Policies` : A policy is an JSON file in AWS that, when associated with an identity or resource, defines their permissions.
    - Inline : Assign policy individually
    - Managed : Assign different identities to single policy.

- `ARN` : Amazon Resource Name
    - arn:partition:service:region:account-id:resource-id
    - arn:partition:service:region:account-id:resource-type:resource-id
    - arn:partition:service:region:account-id:resource-type:resource-id

#### Permission Priority
>*Explicit Deny > Explicit Allow > Default(Implicit Deny)*

#### IAM Security Tools
- **IAM Credentials Report** (account-level) - lists all of the users and the status of their credentials
- **IAM Access Advisor** (user-level) - shows the service permissions granted to a user and when those services were last accessed.


# `Elastic Compute Cloud`
It's just VMs with on-demand price and scalable compute capacity option.
>You will be charged for the AMI, Instance Type, EBS Volume, Networking, IOs, Snapshots.

- Infrastructure-as-a-service.
- Configuration options :
    - OS
    - Compute Power & Cores(CPU)
    - RAM
    - Storage space 
        - Network Attached(EBS & EFS)
        - Hardware Attached(Instance Store)
    - Network Card : speed of the card, kind of public IP
    - Firewall rules(security groups)
    - Bootstrap Script(EC2 user data)

>Note: Apache HTTPD is an HTTP server daemon produced by the Apache Foundation. It is a piece of software that listens for network requests (which are expressed using the Hypertext Transfer Protocol) and responds to them.  It is open source and many entities use it to host their websites.  Other HTTP servers are available (including Apache Tomcat which is designed for running server side programs written in Java (which don't use CGI)).  CGI is a protocol that allows an HTTP server to use an external piece of software to determine how to respond to a request instead of simply returning the contents of a static file. Many HTTP servers support the CGI protocol.  You can use CGI without an HTTP server, but this typically has few uses beyond allowing a developer to perform command line testing of the CGI program. (You certainly can't interact with it directly from a web browser).
- EC2 instance types:
    - General Purpose(T)
    - Compute Optimized(C)
    - Memory Optimized(R,X,Z)
    - Accelerated Computing()
    - Storage Optimized(I,D,H)
    - Instance Features
    - Measuring Instance Performance

Ex: m5.2xlarge 

m -> instance class
5 -> generation
2xlarge -> size within the instance class

## Security Groups
- Controls inbound and outbound traffic
- Only contains allow rules
- Acts as firewall for EC2 instances
- Regulates:
    - Access to ports
    - Authorized IP ranges
    - Controls of Inbound Network(from other to instance)
    - Controls of Outbound Network(from instance to other)
- Can be attached to multiple instances
- If your application is not accesible(or timed out), then it's most probably security group issue
- And if it's "connection refused" then it's application error or it's not launched
- All inbound traffic is blocked by default
- All outbound is authorized by default

```
Classic Ports:
22 - SSH 
21 - FTP
22 - SFTP
80 - HTTP
443 - HTTPS
3389 - RDP
```

**Creating KeyPair for ec2 does not cost anything*

**Pricing Options** : 
- *On-Demand Instance* : 
    - No upfront payment or long-term commitment, just pay by the hour.
    - Used for Application with short-term , spiky or unpredictable workloads that cannot be interrupted.
- *Reserved Instances* (1 & 3 Y):
    - Reserved instance : for long workloads
    - Convertible Reserved instance : long workloads with flexible instances
- *Savings Plan* (1 & 3 Y): Commitment to an amount of usage, long workload
- *Spot Instance* : short workloads, cheap, can lose instances (less reliable).
- *Dedicated Hosts* : 
    - Book entire physical server control instance placement.
    - An Amazon EC2 Dedicated Host is a physical server fully dedicated for your use.
    - Amazon EC2 Dedicated Hosts allow you to use your eligible software licenses from vendors such as Microsoft and Oracle on Amazon EC2, so that you get the flexibility and cost effectiveness of using your own licenses, but with the resiliency, simplicity and elasticity of AWS. 
- *Dedicated Instances* : no other customers will share your hardware.
- *Capacity Reservations* : reserve capacity in a specific AZ for any duration

**Price Comparison** :
Example – m4.large – us-east-1
| Price Type                             | Price(per hour)     |
| -------------------------------------- | ------------------- |
| On-Demand                              | $ 0.10              |
| Spot-Instance (Spot Price)             | $ 0.10              |
| Reserved Instance (1 Year)             | $ 0.10              |
| Reserved Instance (3 Year)             | $ 0.10              |
| EC2-Savings Plan (1 Year)              | $ 0.10              |
| Reserved Convertible Instance (1 Year) | $ 0.10              |
| Dedicated Host                         | $ 0.10              |
| Dedicated Host Reservation             | $ 0.10              |
| Capacity Reservation                   | $ 0.10              |

## Instance States
| Instance state  | Description                                                                                                                                                                                 | Billing                                                           |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------- |
| `pending`       | The instance is preparing to enter the `running` state. An instance enters the pending state when it launches for the first time, or when it is started after being in the `stopped` state. | Not billed                                                        |
| `running`       | The instance is running and ready for use.                                                                                                                                                  | Billed                                                            |
| `stopping`      | The instance is preparing to be stopped or stop-hibernated.                                                                                                                                 | Not billed if preparing to stop. Billed if preparing to hibernate |
| `stopped`       | The instance is shut down and cannot be used. The instance can be started at any time.                                                                                                      | Not billed                                                        |
| `shutting-down` | The instance is preparing to be terminated.                                                                                                                                                 | Not billed                                                        |
| `terminated`    | The instance has been permanently deleted and cannot be started.                                                                                                                            | Not billed                                                        |
