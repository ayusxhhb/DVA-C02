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

- `Groups` : Collection of similar people with shared permissions & users will inherit permissions from the group.

    **It's best to inherit permissions from groups so that you don't have to manage them individually**

- `Roles` : Any software service that needs to be granted permissions to do its job. 

    It's similar to IAM user, however instead of being assiciated with one person , a role is intended to be assumed by anyone who needs it. Roles are temporary. Also generate temporary credentials for the user.

    *Two types of policies are attached to a role :*
    - Trust Policy : Who can assume this role?
    - Permission Policy : What can they do after assuming the role?

- `Policies` : A policy is an JSON file in AWS that, when associated with an identity or resource, defines their permissions.

#### Permission Priority
>*Explicit Deny > Explicit Allow > Default(Implicit Deny)*

#### IAM Security Tools
- **IAM Credentials Report** (account-level) - lists all of the users and the status of their credentials
- **IAM Access Advisor** (user-level) - shows the service permissions granted to a user and when those services were last accessed.

# `Elastic Compute Cloud`

It's just VMs with on-demand price and scalable compute capacity option.

**Pricing Options** : 
- *On-Demand Instance* : 
    - No upfront payment or long-term commitment, just pay by the hour.
    - Used for Application with short-term , spiky or unpredictable workloads that cannot be interrupted.
- *Spot Instance* : 
    - Uses spare EC2 capacity that is available for less than the On-Demand price.
    - The Spot price of each instance type in each Availability Zone is set by Amazon EC2, and is adjusted gradually based on the long-term supply of and demand for Spot Instances.
    - Your Spot Instance runs whenever capacity is available.
        >To terminate your spot instances you need to first cancel the spot request and then terminate the instances.
- *Dedicated Hosts* : 

```
                            |<----------- Start <-------------------- Stop
                            |                                           |
                            |  |-------- Interrupt -------------------| |                            
                            |  |                                      | |
           Create req ---> Spot req -----------> Launch-instances ---> VMs
                            |                                           |
                            |                                           |
                         Request                                     Interrupt
                         failed
```

